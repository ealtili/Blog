So let's get started with IAM.
So first, there's a lot of things you should know
about IAM already so I'm not gonna go over the basics,
I will just try to remind you what it is.
So, users have long-term credentials
and therefore you're AWS users.
You can group them, and you can also define roles
which are going to be short-term credentials.
And then we use the STS service
to endorse these roles and get credentials
that are going to be temporary out of those
to do the actions that the roles are authorized to do.
A specific kind of role we've been seeing all along
in the courses is the EC2 Instance Role,
where it's going to use the EC2 metadata service
to get these short-term credentials on an EC2 Instance.
And you can assign only one role at a time for the instance.
But this way your instance can access, for example,
an S3 bucket or within an OTB table and so on.
There's also service roles,
which are assigned to services directly,
so that if you have a service such as API Gateway
or CodeDeploy that needs to do an action
on an auto-scanning group or another function
or something else, then that service needs to have a role
and that role needs to be able
and provisioned to do all the actions it needs.
Okay, and then finally we have Cross Account roles
and these roles are going to be really helpful
in case you need to access another account
to perform actions in that other account.
You never share users credentials Cross Account,
you always allow to assume roles
and we'll see this in detail in this course.
Now you have policies in IAM
and they will define what a roll or user can do.
And so you have three kinds.
You have AWS Managed, which is policies defined by AWS
that are going to be changing over time maybe
but they will do something specific.
Customer Managed, which is you creating these policies
and you can assign them to multiple users
or multiple roles and you can make
them evolve over time, version them.
Or Inline Policies, which are going to be policies assigned
to one specific user or one specific role
and you can make them evolve over time
but you cannot share them across users
or across roles.
And finally we'll do a discussion on Resource Based Policies
so this is when you have an S3 bucket policy
or an SQS queue policy and so on,
which would allow us to perform
some really interesting patterns
and we'll see this in this lecture.
So what does an IAM Policy look like?
Well, first of all, it's going to be adjacent documents
and they will have four things or five things.
Effects, action, resources, conditions
and sometimes in it policy variables.
We'll do a deep dive in all of those.
But the idea is that, adjacent policy looks like this
and then we have some statement, for example,
allow ec2 attach volume,
ec2 detach volume on the resource,
which is all ec2 instances
and the condition is
StringEquals resource tag department developments
that means that only the ec2 instances tagged
with this tag can be attached
or detached to a volume and so on.
So this is quite specific and you can get very very crazy
with IAM policies but this is how all
of AWS works and we know this already.
If there's an explicit deny in your IAM policy
then you will have precedence over any allow
and so that means that inclusive deny's
always have the highest priority
and we'll have a lecture exactly to understand
how IAM policies and everything else are evaluated in order.
Okay, so the best practice, we know this already,
is to use the least privilege for maximum security.
That means that you need to make sure
that the IAM policies are allowed
just to do what they need to be doing and not more.
Some tools we can use to make sure that this is the case,
there is IAM access advisor.
We can see all the permissions
you have granted to an IAM policy
and the last time each
of these permissions was last accessed.
So in case, you have a policy
or something that was not used for a year,
maybe it's worth removing it from the IAM policy
to ensure there's less privilege.
Okay, another one is going to be Access Analyzer
and this is to analyze resources
that are shared with external entities,
for example, S3 buckets and this will allow you to look
at if other accounts have access to your S3 buckets.
Maybe there's something you're not expecting here
and you want to make sure to look there on the S3 buckets.
Okay, if you're not very familiar with IAM policy,
I would encourage you to go to this URL
to make sure you look at a few of them
and understand them better
but I would assume that by now you know
what they look like already and how they work.
A few common IAM policies we'll get across
is going to be the AdministrativeAccess
which is meaning that everything
should be allowed on any resource
and so this is a policy you have to specify.
So by default if you set nothing in an IAM policy
then everything will be denied
but if you add that statement,
which is allow action*resource*
then everything will be allowed

and that will give you administrative access

and because this is something very common

for administrators, AWS provides this as a manage policy.

Another very interesting policy, a bit less privileged,

is called AWS PowerUserAccess.

And so in the first front the effect is allow,

not action on IAM* organizations

and account* for resource*.

So that means that it's not going to allow anything

to be done on IAM, organizations and accounts

and also it's going to allow still

a few IAM actions such as,

iam:CreateServiceLinkedRole,

iam:DeleteServiceLinkedRole,

iam:ListRoles,

organizations:DescribeOrganization

and account:ListRegions.

So you may be asking me why on the left-hand side

is NotAction used and not just effect denied.

So if you use effect deny

and then you specify these three actions,

and then you specify effect allow

and these five actions will automatically be denied

because there will be an explicit deny

on the left-hand side.

So here we have a very interesting use case,

in case we don't want to deny everything in there

because we want to explicitly allow a few things in there

then we can use the NotAction instead of deny

to allow for these two things to co-exist together.

Okay, next.

Conditions in IAM policies.

So this is what a condition would look like.

So we have a condition operator,

a condition key and a condition value

and so we can do a lot of crazy things with conditions.

The first one is going to be a string operator,

so for example, you're saying I want my principle tag

to be job category IAM user admin.

So this is when you look at tags.

Or you can look at, for example, on this prefix

for an S3 policy, to say that we want

the users to only access a specific home directory.

There's numeric operators,

where we can look at is it equal, equal than

or not equal and so on.

Dates, to look at compare dates.

This is very helpful when you want

to provide temporary access to a specific service.

Booleans, which is going to be really helpful,

for example, if you want to evaluate SSL,

such as secure transport true

and also when you want to look at MFA

using multi factor authentication present true.

There's also an IP address condition

or a not IP address condition

and these conditions will be really helpful

for buckets, S3 buckets policies for example

or any kind of policy when you want to ensure

that only a specific kind of source IP

can access a service.

So, we can have

an "{Ipaddress":{aws:SourceIp":"203.2.113 0/24"}}.

And finally we can look at ArnEquals, ArnLike

and the null condition,

which we're checking if something is null or not.

Now right now, I just want to reassure you,

you don't have to remember all of these things okay.

I just want to show you these things exist,

so that you know you can have very specific IAM policies,

such as if the exam is asking you to use an IAM policy

as an answer to your problem,

this would be possible instead of using,

for example, a custom script.

But you don't need to remember exactly all these things,

the exact conditions and so on.

Okay next, there is policy variables and tags

and these are really really powerful.

One we've seen is ${aws:username}

and this is most commonly used, for example,

in an S3 bucket policy.

So we have,

"Resource",["Am:awss3:::mybucket/${aswusername}/*"]

and what this does is that it will allow your username

to perform actions, all the actions on just the prefix

that starts with aws username.

And so that means that every user will have

it's own little directory in your S3 bucket

and that is a very common usage for this.

There's AWS specific policy variable in tags, for example,

current time, token issue time, principle type,

secure transport which is for SSL, source Ip,

user ID and source instance ARN

and these are provided by AWS.

There's service specific policy roles and tags,

for example, S3 prefix, max keys, sns Endpoint, sns Protocol

and finally you can have tag based policy variables,

for example if you use

iam:ResourceTag/key-name,aws:PrincipleTag/key-name

and so one.

So you can start having some really powerful IAM policies

and this why I would encourage you

to use the link from before to just look at a few of them

and try to understand them, so you can really see

the whole range of what can be done in IAM.

Okay, more importantly and more exam focused,

is going to be the difference if IAM roles

in resource based policies

and it may not be very clear exactly what the difference is,

but there is a very very clear difference.

So we have two options right,

we can attach a policy to a resource, for example,

a S3 bucket versus attaching of using a role as a proxy.

So here is an example,

we have a user in account A and he's trying to access

an S3 bucket in account B.

Now there's two ways of doing this, right.

The first one is to use a role in account B

and we'll be assuming that role from account A.

And once you've assumed the role in account B,

the role will have the necessary permissions

to access the S3 buckets and that works.

Another option is to use an S3 bucket policy

and in the S3 bucket policy,

we're saying okay, user in account A can access

my Amazon S3 bucket in account B.

In both these solutions, it will allow user account A

to access the Amazon S3 bucket in account B.

But there is a very, very big difference.

What is it?

It is that when you assume a role, would it be a user,

an application or a service,

you're going to give up all your original permissions

and you're going to take the permissions

assigned to the roll.

So that means that before, when we do assume the role

in account B, the orange situation,

then the user in account A gives up all his permissions

in account A, while it's been assuming a role in account B.

So what this means, that when you're using

a resource based policy otherwise

the principle doesn't have to give up any permissions.

So if we have a more complex example,

where the user in account A needs to scan a DynamoDB table

in account A and dump it in an S3 bucket in account B,

save very very regularly.

In this case, if we use a role to assume a role

in account B, then we will not be able

to scan that DynamoDB table in account A anymore

because we've been giving up our permissions

and that won't fit the use case.

So in this case, much better is to use an S3 bucket policy

which would allow us to access the S3 bucket,

yes and also access the DynamoDB table in account A.

So where are these things reported,

these resource based policies.

There's quite a few services

but some of the main ones you should know is,

Amazon S3 Buckets, SNS topics and SQS queues.

Okay, so I hope everything make sense in this lecture,

just a quick revision on IAM

to show you the whole possibilities

and very important, you need to understand

the difference between IAM roles

and resource based policies.

And that's it from me.

I will see you in the next lecture.


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1OTc4NzY4Myw3MzA5OTgxMTZdfQ==
-->