
## AWS Accounts

Let's have a look at AWS accounts. 

What is an AWS account ?
What benefits it provides ?

An AWS account is a container for identities and AWS resources.

Identities are things like users. So these things which lets you to log in to systems such as AWS.

So an AWS account contains users which you log in with
and resources which you provision inside of that account.

The email address that you provide when creating an AWS account is used to create a special type of identity within the AWS account known as the account root user.
Every AWS account has an account root user. Account root user has full control over that one specific AWS account and any resources which are created inside it. Account root user can't be restricted and will always have full access to everything within that specific AWS account it belongs to. For that reason it is the most important account to to protect.

Just to emphasize the importance one more the account root user has full control over one specific AWS account   can't be restricted.  

You can create additional identities inside the AWS account and they can be restricted.

Identity and Access Management service (known as IAM)  can create other identities inside the account.

Types of identities:

- IAM users
- IAM groups
- IAM roles

It is important to know that all of these IAM identities has no access to the AWS account, unless they are given
some (like full or limited) access rights over one specific AWS account.

So just like the account root user, the IAM service is also
dedicated to your account.

So unless you specify otherwise,

any IAM identities created in my account

won't be able to access your account,

only identities which you
create inside your account

and then grant access

will be able to access resources

within your AWS account.



I'll talk about how you can do

cross account permissions.

But for this point

just think of AWS accounts as containers,

only things within the accounts

can access anything else

within that same account.

And another key thing to remember

is that with the exception
of the account root user,

any IAM identity starts off

with no permissions.

You have to explicitly grant permissions

to any identities managed
by the IAM service.

Now, another concept which I want you

to be really familiar with

is the boundary of the account.



AWS accounts are really good

at containing any damage caused

within those accounts.

So things such as

an inexperienced system
administrator doing something silly

or a bad actor attempting to intentionally

harm your account.

If the credentials for the
account root user are leaked,

then these could be used
to delete everything

inside that one specific AWS account.

And if your entire business

runs from that one single account

then this can be really, really bad.

However, if you create
separate AWS accounts

for different uses

maybe a development account,

a test account

and a production account,

then you can limit the damage.

If you have any credential leakage,

or if you have any system admins

doing any silly mistakes

causing resources to be deleted

then generally these will be isolated

to that one specific AWS account.

You can also create AWS accounts

for different teams within your business

or even different products
that your business sells.

AWS accounts are great
for keeping bad things

inside one specific part
of your AWS environment.

They're also really good

for keeping things
outside of that boundary.

By default, all access to
an AWS account is denied.

So unless you configure otherwise

no external identity

is allowed access to an AWS account.

The exception to this of course

is the account root user of that account

who always has full control.

Now, this means that
any external identities,

for example, external
users are denied by default

if they attempt to
access your AWS account.

Now external identities
can be granted access

such as John in the middle

if you explicitly want
to allow this access.

But the default security stance

is that unless you
explicitly allow something

then no access is allowed
to your AWS account.

So that's AWS accounts.

They're a crucial part

of the AWS product set

and understanding them

to really instinctive level

is important.

Whether you're a solutions architect,

a developer,

assist admin or a DevOps engineer,

you need to be comfortable

with how to use AWS accounts effectively.



Now, I'm strongly recommending

that you create brand new AWS accounts.

Don't make the mistake of trying to use

your existing AWS account or accounts

because they're likely to be

in a pretty bad state.

The longer an AWS account exists

the more potential there
is for misconfiguration.

And so it's much better
to use brand new accounts



I want you to learn how
to configure all of this

in a correct way

using best practices.



I'll even show you a trick

so that you can use one email account

to create all of the unique
email addresses needed

for these multiple accounts.

So it shouldn't take that
much in the way of effort.





> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjQxMTEzMTk1LC0yNTQ0NTM5NDNdfQ==
-->