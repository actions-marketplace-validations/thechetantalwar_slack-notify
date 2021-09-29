## Slack Notify - Github Actions
---

- A Github Action to send Job Status to Slack Channel

## Usage
- In the below stated example, separate job for `Notifications` has been created which will send the notification to the SLACK CHANNEL irrespective of the build job status.
- Message to be sent can be customized as per the need, you can include other properties as well using Github Contexts.
- In order to use this, all you need is to create a Webhook in your Slack Channel, follow <a href="https://api.slack.com/messaging/webhooks#create_a_webhook">this link</a> for info about creating a one.
- Once you have created the webhook, just add the Webhook Address in secrets and utilise in your Action File in this manner `${{ secrets.SECRET_NAME }}`.
- Sample Usage
    
    ```
    name: Pipeline Name

    on:
    push:
        branches: [ branch_name ]
    pull_request:
        branches: [ branch_name ]
    jobs:

    build:
        runs-on: ubuntu-latest
        steps:
        - name: Demo Step
          run: docker push ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:$GITHUB_RUN_NUMBER
    notify:
        runs-on: ubuntu-latest
        if: always()
        needs: build
        steps:
        - name: Send a Notification
            id: notify
            uses: thechetantalwar/slack-notify@v2
            with:
            slack_webhook_url: ${{ secrets.SLACK_HOOK }}
            message: "Github Action Build Number ${{ github.run_number }} Completed for ${{ github.repository }} and the outcome is  ${{ needs.build.result }}."
    ```

- The above stated sample will send a message like below
    ![Sample Notification](https://github.com/thechetantalwar/slack-notify/blob/master/sample.png?raw=true)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)