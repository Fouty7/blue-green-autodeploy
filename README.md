# Automated Blue-Green Deployment with CircleCI

This project utilizes a CI/CD (Continuous Integration/Continuous Deployment) configuration file for automating the build, test, deployment, and maintenance processes. The CI/CD pipeline is implemented using CircleCI and consists of multiple jobs and workflows. Here is an overview of the project structure and how the CI/CD pipeline is set up.

## CI/CD Configuration

The CI/CD configuration file (`config.yml`) is written using CircleCI 2.1 syntax. It defines various jobs, workflows, and commands necessary to automate the software development lifecycle. The following sections describe the different components of the configuration file.

### Orbs

- `slack`: CircleCI Slack orb version 4.10.1 is utilized for integrating with Slack and sending notifications.

### Commands

- `destroy-environment`: Destroys the backend and frontend cloudformation stacks for a given workflow ID.
  - Parameters:
    - `WorkflowID`: The ID of the workflow. By default, it uses the first seven characters of the CircleCI workflow ID.

- `revert-migrations`: Reverts migrations for a given workflow ID.
  - Parameters:
    - `WorkflowID`: The ID of the workflow. By default, it uses the first seven characters of the CircleCI workflow ID.

### Jobs

The following jobs are defined:

- `build-frontend`: Builds the frontend application.
- `build-backend`: Builds the backend application.
- `test-frontend`: Runs frontend tests.
- `test-backend`: Runs backend tests.
- `scan-frontend`: Performs security audit and analysis for the frontend.
- `scan-backend`: Performs security audit and analysis for the backend.
- `deploy-infrastructure`: Deploys the backend and frontend infrastructure using AWS CloudFormation.
- `configure-infrastructure`: Configures the server and installs dependencies using Ansible.
- `run-migrations`: Runs migrations for the backend.
- `deploy-frontend`: Deploys the frontend artifacts to an S3 bucket.
- `deploy-backend`: Deploys the backend artifacts using Ansible.
- `smoke-test`: Performs smoke tests for both frontend and backend.
- `cloudfront-update`: Updates the CloudFront distribution.
- `cleanup`: Cleans up the resources associated with the previous workflow.

### Workflows

The project includes a single workflow named `blue-green-deployment`, which orchestrates the different jobs in a specific order:

1. `build-frontend` and `build-backend` run in parallel.
2. After successful builds, `test-frontend` and `test-backend` jobs are triggered.
3. Following successful tests, `scan-frontend` and `scan-backend` jobs are executed.
4. Once the scanning is complete, `deploy-infrastructure` deploys the backend and frontend infrastructure using AWS CloudFormation.
5. After infrastructure deployment, `configure-infrastructure` configures the server using Ansible.
6. Next, `run-migrations` runs migrations for the backend.
7. Upon successful migrations, `deploy-frontend` and `deploy-backend` deploy the frontend and backend artifacts, respectively.
8. After deployment, `smoke-test` performs smoke tests to validate the deployed application.
9. `cloudfront-update` updates the CloudFront distribution.
10. Finally, `cleanup` cleans up resources associated with the previous workflow.

## Usage

To utilize the CI/CD pipeline and automate the software development lifecycle, follow these steps:

1. Ensure CircleCI is set up and integrated with your repository.
2. Configure the required environment variables and secrets in the CircleCI project settings.
3. Customize the configuration file as per your project requirements, such as changing image versions, paths, or adding additional steps.
4. Commit and push the `.circleci/config.yml` file to your repository.
5. CircleCI will automatically detect the changes and trigger the defined workflows based on the specified conditions.

Feel free to modify the configuration file, jobs, or workflows to suit your specific project needs.

For more information about CircleCI and its configuration, please refer to the official CircleCI documentation.

**Note:** This README provides a high-level overview of the CI/CD pipeline and its components. For detailed information on each job and workflow step, refer to the comments and commands in the `config.yml` file.
