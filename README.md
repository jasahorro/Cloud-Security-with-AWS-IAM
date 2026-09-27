<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](https://nextwork.ai/projects/51a5fd24-7458-58a6-8653-9b90119a0e4a)

**Author:** Maria Jasmin Ahorro  
**Email:** jasminahorro23@gmail.com

---

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_1c864649)

## Introducing Today's Project!

### Project overview

In this project, I will demonstrate how to set up AWS IAM policies, user groups, and account aliases to enforce least-privilege access across production and development environments. I'm doing this project to learn cloud security fundamentals, user authentication, and how to control access to EC2 instances safely.

### Tools and concepts

Services I used were Amazon EC2 and AWS Identity and Access Management (IAM).

Key concepts I learnt include tag-based access control (ABAC), which allowed me to tag resources with attributes like Env = development to establish clear permission boundaries across different environments. I also learned how to implement least-privilege policy design by authoring custom JSON IAM policies with explicit Allow and Deny rules, restricting administrative actions to specific operational scopes. Additionally, I applied group-based permission management by grouping users into IAM User Groups to maintain consistent security policies across multiple identities. To simplify access, I customized account configuration by establishing an AWS Account Alias, creating a user-friendly console sign-in URL. Finally, I practiced safe policy testing by leveraging the AWS IAM Policy Simulator to validate permission logic and condition statements before applying changes to live workloads.

### Project reflection

This project took me approximately 20-30 minutes.

The most challenging part was understanding and configuring the JSON policy condition block to properly restrict access based on resource tags.

It was most rewarding to successfully log in as the IAM user and see the tag-based access restrictions working in real time when attempting to manage the EC2 instances.

## Tags

### What I did in this step

In this step, I will launch two Amazon EC2 instances for production and development because we need to increase compute capacity and prepare isolated environments to safely test access controls.

### Understanding tags

Tags are key-value pairs assigned to AWS resources to label, organize, and manage them. They are useful for categorizing resources by environment (such as production or development), tracking costs, filtering search results, and enforcing access control policies based on tag values.

### My tag configuration

The tag I’ve used on my EC2 instances is called Env. The values I’ve assigned for my instances are production for the production instance and development for the development instance. Tags help organize resources, track costs, and enforce attribute-based access control policies in AWS.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_2e0e5a5d)

## IAM Policies

### What I did in this step

In this step, I will create a new IAM policy with JSON permissions because we need to grant our intern access to the development EC2 instance while restricting access to the production environment.

### Understanding IAM policies

IAM Policies are documents (written in JSON) that define permissions in AWS. They specify what actions users, groups, or roles can or cannot perform on specific AWS resources, and under what conditions.

### The policy I set up

For this project, I’ve set up a policy using JSON because it allows direct, precise control over IAM permissions and condition statements, making it easier to define resource-level access based on tags.

### Policy effect

I’ve created a policy that allows full access to EC2 actions only for resources tagged with Env = development, allows describing all EC2 resources, and denies creating or deleting tags on any EC2 resource.

### Understanding Effect, Action, and Resource

The Effect attribute specifies whether the policy allows or denies the requested permissions. The Action attribute defines the specific AWS service operations or tasks that are permitted or restricted. Finally, the Resource attribute identifies the individual AWS resources to which these actions apply, ensuring fine-grained access control across your AWS environment.   

## My JSON Policy

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_1c864649)

## Account Alias

### What I did in this step

In this step, I will create a custom AWS Account Alias because it replaces our numeric AWS account ID with a friendly, easy-to-remember sign-in URL for new users.

### Understanding account aliases

An account alias is a friendly, customized name for your AWS account that replaces the default 12-digit AWS account ID in your sign-in URL, making the login link easier to remember and share with team members.

### Setting up my account alias

Creating an account alias took me less than a minute. Now, my new AWS console sign-in URL is [https://nextwork-alias-jasahorro.signin.aws.amazon.com/console/](https://nextwork-alias-jasahorro.signin.aws.amazon.com/console/).

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_0eb4439b)

## IAM Users and User Groups

### What I did in this step

In this step, I will set up a dedicated IAM group and IAM user for our new intern because it allows us to easily manage and apply consistent permissions to the intern while ensuring they can safely log in to access the development environment.

### Understanding user groups

IAM user groups are collections of IAM users that allow you to manage permissions for multiple users at once by attaching policies to the group rather than to each user individually.

### Attaching policies to user groups

I attached the policy I created to this user group, which means any IAM user added to this group will automatically inherit the policy's permissions, granting them full access to the development environment (Env = development) while restricting access to production resources.

### Understanding IAM users

IAM users are identities created within an AWS account that represent individual people or applications, each with their own credentials (like passwords or access keys) and specific permissions to interact with AWS resources.

## Logging in as an IAM User

### Sharing sign-in details

The first way is by downloading the .csv file containing the credentials and sharing it securely with the user. The second way is by copying or emailing the Console sign-in instructions directly from the AWS Console.

### Observations from the IAM user dashboard

Once I logged in as my IAM user, I noticed Access Denied errors across multiple console widgets and services, but I was able to view and manage the development EC2 instance.   This was because our custom IAM policy only granted explicit permissions to actions on resources tagged with Env = development and ec2:Describe* operations, while restricting access to all other AWS services and production resources

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_6f2ab446)

## Testing IAM Policies

### What I did in this step

In this step, I will log into the AWS console using the intern's IAM user credentials and test access to the EC2 instances because we need to verify that our policy and group permissions correctly grant access to the development instance while blocking access to the production instance.

### Testing policy actions

I tested my JSON IAM policy by attempting to stop both the development and production EC2 instances while logged in as the new IAM user. Stopping the development instance succeeded, whereas stopping the production instance failed with an Access Denied error, confirming that the resource-tagging condition in my policy was working properly.

### Stopping the production instance

When I tried to stop the production instance, I received an Failed / Access Denied error. This was because our IAM policy strictly restricts EC2 control actions (like stopping instances) to resources tagged with Env = development, explicitly blocking modifications to production resources.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_0e7a9d6a)

### Stopping the development instance

Next, when I tried to stop the development instance, the action succeeded without any errors. This was because our IAM policy explicitly allows EC2 management actions on resources tagged with Env = development.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_1811801c)

## IAM Policy Simulator

To extend my project, I'm going to test my custom IAM policy in the AWS IAM Policy Simulator. I'm doing this because it allows me to simulate and verify action permissions without needing to modify live, running EC2 instances or risk disrupting active environments.   

### Understanding the IAM Policy Simulator

The IAM Policy Simulator is a tool provided by AWS that allows you to test and troubleshoot IAM policies in a safe, simulated environment.

It's useful for testing the effect of policies before applying them to real users or resources, helping you verify that permissions are correctly granted or denied without risking changes to live infrastructure or affecting running operations.

### How I used the simulator

I set up a simulation for the ec2:StopInstances action on my custom IAM policy in the IAM Policy Simulator. The results were denied by default until I configured the simulation context with the ec2:ResourceTag/Env tag set to development, which changed the result to allowed. I had to adjust the request context settings to include the resource tag so the simulator could evaluate the policy's condition block accurately.

![Image](https://nextwork.ai/overjoyed_magenta_proud_yak/uploads/51a5fd24-7458-58a6-8653-9b90119a0e4a_069d8a621)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/51a5fd24-7458-58a6-8653-9b90119a0e4a)*
