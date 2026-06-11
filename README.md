<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)

**Author:** Khaled Ashraf  
**Email:** khaledashrafzzz583@gmail.com

---

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_1c864649)

---

## Introducing Today's Project!

### Project overview

We are using the AWS Identity and Access Management (IAM) service to control access to AWS resources. Specifically, we will launch EC2 instances and then manage who has the authorization to access them by creating IAM policies, an IAM user group, an IAM user for a new intern, and a friendly AWS Account Alias for easier login.

### Tools and concepts

In this project, I learned how to deploy and secure cloud infrastructure using two core AWS services:

    Amazon EC2: I learned how to launch virtual servers, select appropriate AMIs and instance types, and organize infrastructure using custom Tags (specifically labeling instances as "production" or "development").

    AWS IAM (Identity and Access Management): I learned how to enforce the principle of least privilege by writing custom JSON Policies. I also learned how to scale permission management by attaching those policies to IAM User Groups rather than individual IAM Users, how to create a custom AWS Account Alias for streamlined team logins, and how to safely validate permissions using the IAM Policy Simulator without risking disruption to live resources.

### Project reflection

It took me approximately 1 hour to complete the full project, including testing the permissions in the secret mission and filling out the customized documentation.

---

## Tags

### What I did in this step

We are launching two Amazon EC2 virtual servers (instances) to increase computing power. We are setting them up with specific tags to differentiate them: one will be tagged for the "production" environment, and the other for the "development" environment.

### Understanding tags

Tags are like custom labels you attach to your AWS resources to help organize them. They are highly useful for filtering and identifying specific resources quickly, managing cost allocation across different departments, and—most importantly for this project—applying specific IAM security policies based on environment types.

### My tag configuration

For the production instance, we assigned the following tags:

    Name: orange-prod-khaled

    Env: production

For the development instance, we assigned the following tags:

    Name: orange-dev-khaled

    Env: development

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_2e0e5a5d)

---

## IAM Policies

### What I did in this step

We are creating a custom IAM policy written in JSON. This policy is designed to grant access to start, stop, and describe EC2 instances that are specifically tagged for the "development" environment, while explicitly denying the ability to create or delete tags so the rules can't be bypassed.

### Understanding IAM policies

IAM policies act as rules that define exact permissions. They are attached to IAM users, groups, or roles, and they state exactly what actions those identities are allowed or denied from taking on specific AWS resources.

### The policy I set up

I set up the IAM policy using the JSON method. I switched the policy editor to JSON and pasted in the specific JSON script to define the Allow and Deny rules for the instances.

### Policy effect

The effect of this policy is that it allows a user to interact with (start, stop, etc.) EC2 instances only if those instances have the specific tag "Env = development". It also allows them to describe (view) all instances, but explicitly denies the ability to create or delete tags on any instances, preventing users from bypassing the security rules by simply changing a production instance's tags.

### Understanding Effect, Action, and Resource

Effect: This dictates the outcome of the policy statement and can be set to either Allow or Deny. It indicates whether you are granting permission or explicitly blocking it (and Deny always takes priority).

Action: This is a list of the specific AWS operations that the policy allows or denies (for example, ec2:* means all possible actions on EC2, or ec2:Describe* for just viewing them).

Resource: This specifies exactly which AWS resources the policy applies to. Using * means it applies to all resources that fall within the scope of the rule or condition block.

---

## My JSON Policy

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_1c864649)

---

## Account Alias

### What I did in this step

We are creating an AWS Account Alias to simplify the login process. By default, the login URL uses a long, random AWS account ID number. The alias replaces that number with a user-friendly, memorable name so the new intern can log in easily.

### Understanding account aliases

An Account Alias is a custom, friendly name for your AWS account that replaces your default, hard-to-remember AWS account ID number. Instead of sharing a login URL filled with random digits, you can create an alias so the sign-in URL is clean, user-friendly, and much easier to share with new team members.

### Setting up my account alias

It only took a few seconds! The process is incredibly fast—it just required navigating to the IAM Dashboard, clicking "Create" under the Account Alias section, typing in the custom name (like main-user-khaled-1), and saving it.

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_0eb4439b)

---

## IAM Users and User Groups

### What I did in this step

We are setting up a dedicated IAM User Group for interns and attaching our development policy to it so we can manage all intern permissions from one place. Then, we are creating a specific IAM User account for the new intern with Console access and adding them to that group.

### Understanding user groups

An IAM user group is a collection of IAM users. It allows administrators to attach permission policies to the group as a whole rather than assigning them to each user individually. This makes managing permissions much easier and ensures consistency across team members with similar roles.

### Attaching policies to user groups

Attaching the policy directly to the user group means that any IAM user added to that group automatically inherits all the permissions defined in the policy. Instead of applying the OrangeDevEnvironmentPolicy to every single engineer one by one, you apply it once to the orange-dev-group. When the new user (orange-khaled) is added, they instantly get the correct access—simplifying permission management and ensuring consistency across the whole team.

### Understanding IAM users

---

## Logging in as an IAM User

### Sharing sign-in details

### Observations from the IAM user dashboard

As a new user with strictly limited permissions, the AWS dashboard displays "Access denied" on several panels. This happens because the IAM policy only grants access to specific EC2 actions, restricting the user from viewing other general account data or services.

---

## Testing IAM Policies

### What I did in this step

We are logging into the AWS Management Console using the new intern's IAM user credentials to test their permissions. We will try to stop both the production instance and the development instance to verify that the IAM policy successfully blocks production access while allowing development access.

### Testing policy actions

The action performed on both EC2 instances was attempting to stop them by changing their instance state.

Specifically, you tested the IAM policy by:

1. Attempting to stop the production instance (orange-prod-khaled), which was blocked and resulted in an "unauthorized" error.

2. Attempting to stop the development instance (orange-dev-khaled), which was successful and changed the instance status to "Stopping".

### Stopping the production instance

When trying to stop the production instance, an error banner appeared at the top of the screen stating "Failed to stop the instance" due to being unauthorized. This happened because the IAM user (orange-khaled) only has permissions granted by the OrangeDevEnvironmentPolicy, which explicitly restricts actions to instances tagged with "Env = development". Since the production instance did not have that tag, the action was blocked by AWS IAM.

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_0e7a9d6a)

### Stopping the development instance

When trying to stop the development instance, the action was successful! A green success banner appeared, and the instance state changed to "Stopping." This happened because the IAM user was successfully granted permission by the attached policy to perform EC2 actions on any instances that have the "Env = development" tag.

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_1811801c)

---

## IAM Policy Simulator

We are using the IAM Policy Simulator to test and validate user permissions in a faster, more efficient way.

We are running these tests virtually because manually shutting down EC2 instances to test access can be highly disruptive to users and other engineers.

The simulator allows us to safely verify whether actions (like DeleteTags and StopInstances) are allowed or denied based on our policy conditions, completely without affecting real AWS resources.

### Understanding the IAM Policy Simulator

You would use the IAM Policy Simulator to safely test and validate user permissions without actually affecting your live AWS resources. Testing permissions manually by attempting actions—like shutting down EC2 instances—can be highly disruptive to other engineers and end users. The simulator allows you to verify exactly what actions are allowed or denied by your policies in a safe, risk-free environment.

### How I used the simulator

When testing the StopInstances action, the initial result showed as denied because the simulator was checking access against all resources by default. However, once the development tag was specifically added to the simulator's Instance field, the result successfully changed to granted. Additionally, the simulation correctly showed that the DeleteTags action was denied, proving the custom IAM policy was working exactly as intended.

![Image](http://learn.nextwork.org/cheerful_beige_innocent_camel/uploads/aws-security-iam_069d8a621)

---

---
