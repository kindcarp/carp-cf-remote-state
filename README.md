# carp-cf-remote-state
This is a CloudFormation repository to set up the remote state to be used by Terraform code developed in other repositories.

### Files

#### `empty-stack.json`
This is a JSON file that creates an empty CloudFormation stack. I use this to start all my CloudFormation stacks, then update with the JSON or YAML file that contains resources.

When CloudFormation runs into a problem, it rolls back. On initial creation, if there's a problem, CloudFormation rolls back. This causes an interesting dynamic where you can't actually update the stack, and must delete it. By using the `empty-stack.json` file to create your empty CloudFormation stack, you avoid this potential pitfall and don't need to delete the stack to update it.

#### `remote-state.yaml`
This is the YAML file that contains all the resources. It pulls in a few parameters and creates an S3 bucket for your terraform backend. It also creates a DynamoDB table to use for state locking, so multiple users can execute terraform changes without conflicts causing issues.

#### `params.json`
This holds the parameters. The parameters are:
`BucketName`, which much be globally unique per AWS's S3 naming policy.
`Owner`, which is used for a tag on the S3 bucket and DynamoDB table.
`TableName`, the name of the DynamoDB table.
