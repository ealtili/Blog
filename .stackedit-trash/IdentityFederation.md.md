
# AWS Accounts

I want to introduce AWS accounts.

Really understanding
what an AWS account is

and what benefits it provides

is one of the most important things

within AWS.

Many students, especially at the start

confuse AWS accounts

with users inside of those accounts.

And I want you to be a
hundred percent clear

on the difference.

If you've used AWS before,

you might already have an idea

of what AWS accounts are

and how they work.

But understanding accounts

at a really instinctive level

is essential for a solutions architect,

developer

or engineer.

Simple systems which you design

might operate from within

a single AWS account,

but more complex systems

or environments operated
by large enterprises

might use tens or in some cases,

hundreds of AWS accounts.

And for that reason

it's important

that you really understand the features

and benefits provided by AWS accounts.

Now, this might seem pretty basic,

but it's really powerful

when you do fully understand it

and really dangerous to
operate at production scale

if you don't.

So let's jump in and get started.

Now, this is an AWS account.

When you're just starting with AWS,

you might only create one of these,

you might even already have one.

Bigger, more complex
projects or businesses

will generally use many AWS accounts.

And in this course,

you're going to use multiple AWS accounts

for the demo lessons.

And this will help you understand

how businesses actually use AWS

in the real world.

At a high level,

an AWS account is a container

for identities and AWS resources.

Identities is just the
technically correct way

of referring to things like users.

So the things which you use to log in

to systems such as AWS.

So an AWS account contains users

which you log in with

and resources which you provision

inside of that account.

So keep this idea of an AWS
account being a container

in your mind constantly

as you move through the course.

But this is especially important

in this stage of the course

where you're going to be creating

the AWS accounts

that you'll use constantly,

as you move through the content.

When you create an AWS account,

you give the account a name,

you need to provide a unique email address

and you also need to
provide a payment method

which is generally a credit card.

So if we're creating a production account

we might name the account
PROD for production.

We would use a unique email address

which is unique for this
specific AWS account

and provide a credit card
for this production account.

So just to reiterate,

the credit card can be used

for multiple AWS accounts,

but the email address can't,

it has to be unique.

You need one unique email address

for every AWS account.

Now, the email address that you provide

when creating the account

is used to create a
special type of identity

within the AWS account

which is known as the account root user.

So every AWS account has
an account root user.

So the root user of that AWS account.

In this example, if you create
a production AWS account

then the account root user of that account

can only log in

to that one production AWS account.

If you make another AWS account,

let's say a developer account

then that account will have its own

unique account root user

with its own unique email address.

So those account root users are different.

The production account root user

can only access the production account

and the developer account root user

can only access the developer account.

Now, initially the account root user

is the only identity,

the only user created

with an AWS account.

So initially, you have the blank account,

the container

and inside that

is the account root user of that account.

Now, the account root
user has full control

over that one specific AWS account

and any resources which
are created inside it.

Now, the account root
user can't be restricted.

It will always have full
access to everything

within that one AWS account,

which it belongs to.

Now, this is the reason

why we need to be really careful

with the account root user

because if the username

and password ever become known

the results can be disastrous,

because the details can be used

to delete everything
within the AWS account.

The credit card that you provide

when you create the AWS account,

that's set as the account payment method.

So you can create resources
within an AWS account,

and I'll be covering these
throughout the course,

and if those resources
have any billable usage

then that usage is billed

to the account payment method.

In this case,

a credit card.

I'll talk about this later in the course,

but on the whole,

AWS is what's known as
a pay-as-you-consume

or pay-as-you-go platform.

If you use a service within
an AWS account for two minutes

then you pay you for two
minutes of that service.

Certain services include

a certain allocation of
free usage per month,

and this is known as the free tier.

And this is what we're
going to take advantage of

in this course

to keep costs at an absolute minimum.

So now that I've covered
the account root user

and billing within an account,

now I want to touch on the security

as it relates to AWS accounts.

So you know that the account root user

has full control over this
one specific AWS account

and this can't be restricted.

Well, you can create additional identities

inside the AWS account,

which can be restricted.

I'll talk about this more soon,

but this uses a service called

Identity and Access Management,

known as IAM.

And with IAM you can
create other identities

inside the account.

So these can be different
types of identities.

We've got IAM users,

IAM groups

and IAM roles.

And I'll cover these in detail

in the relevant section of the course.

But at this point, it's
important to understand

that all of these IAM identities

start off with no access
to the AWS account,

but they can all be given

full or limited access rights

over this one specific AWS account.

So just like the account root user,

the IAM service is also
dedicated to your account.

So unless you specify otherwise,

any IAM identities created in my account

won't be able to access your account,

only identities which you
create inside your account

and then grant access

will be able to access resources

within your AWS account.

Now later in the course

I'll talk about how you can do

cross account permissions.

But for this point in the course

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

So on screen now,

the orange line around the account,

think of this as a wall.

It can keep things inside the account

from getting out

and also keep things outside the account

from getting in.

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

such as Julie in the middle

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

In the following lessons

you're going to create the AWS accounts

which you'll be using

for the duration of this course.

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

for this course.

I want you to learn how
to configure all of this

in a correct way

using best practices.

So please,

go ahead and create

brand new AWS accounts for this course.

I'll even show you a trick

so that you can use one email account

to create all of the unique
email addresses needed

for these multiple accounts.

So it shouldn't take that
much in the way of effort.

With that being said,

go ahead and complete this lesson.

And then when you're ready,

I look forward to your joining me

in the next lesson

where we're going to get started

on creating the AWS
accounts for this course.



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MjI5MDMzNl19
-->