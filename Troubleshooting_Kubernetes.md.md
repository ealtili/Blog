# Troubleshooting Kubernetes Deployments

Let's look at troubleshooting kubernetes workloads such as pods, deployments and services. Troubleshooting these things is not that complicated. We will look at 
different scenarios and we will see that thought process and the pattern of troubleshooting these things is very
similar. 

 1. **Line Endings Issues**

Line ending issues are the most encountered issues. Mostly occurs because developers works on different operating systems and each operating system has its own way of defining where and how lines ends. Windows uses crlf to determine the end of lines. But when you work on docker containers that run in kubernetes or any kind of a Linux system you have to make sure config files,
scripts and yaml files are set to LF. In the vscode (Visual Studio Code) open the file look at the bottom right corner you will see crlf line endings so just click this and we can toggle back to LF and remember to save
the file.

![](https://i.stack.imgur.com/Bzehj.png)

When you are working with git based source control tool it will automatically check out these files. So for windows developer it will checks out the file and it will automatically switch it back to crlf. If a linux developer
checks it out it will set it to a lf. This can cause problems because sometimes that file, config file or code
can be in a docker container. Which ends up in a production environment and CI/CD can fail and you can have problems in production. To prevent this in the git repository you can create and .gitattributes file and 
inside that file you can target files like *.sh files (shell scripts), *.conf (configuration files) ,  YAML files
and code files to keep things in lf.

![](https://i.stack.imgur.com/j64Es.png)

```
# Change * text=auto to * text=false to disable automatic handling
    
# Handle line endings automatically for files detected as text and leave all files detected as binary untouched.
* text=auto

# Never modify line endings of our bash scripts
*.sh -crlf

#
# The above will handle all files NOT found below
#
# These files are text and should be normalized (Convert crlf => lf)
*.css           text
*.html          text
*.java          text
*.js            text
*.json          text
*.properties    text
*.txt           text
*.xml           text

# These files are binary and should be left untouched
# (binary is macro for -text -diff)
*.class         binary
*.jar           binary
*.gif           binary
*.jpg           binary
*.png           binary

```

Now this will only help about 80% of the cases. A lot of the time files may still end up with the wrong line
endings. When you work with docker you might have source files being copied into container images with the wrong line endings. When you have kubernetes clusters you might have config maps and secrets which are being
uploaded from a developer in this
machine right into the environment also
containing the wrong line endings and
these can cause weird problems now to
show you some of the weirdness you can
see I created a script a basic batch
script I've set the line innings to crlf
in the bottom corner and I'm gonna run a
Debian based container so when I go into
that container and I run that script
look what happens bad interpreter at con
interpret the shebang and we get no such
file or directory not to show you
another Linux environment I created an
Alpine based container and I'm mounting
this exactly the same script in and look
when I execute that one it says not
found now you could be running a Python
application dotnet WordPress site you
could be trying to load some
configuration and you'll get like a file
not found and you'll be pulling your
hair out trying to figure out and
meanwhile it's just a line endings that
you could switch so always keep that
alarm bell in your head about line
ending so the basic strategy I try and
follow is first look at deployments then
look at pods and for both of those two
we're going to look at status and events
and that's generally enough to cover all
most of the scenarios when it comes to
kubernetes and deployment failures so
now we're gonna run through a few
scenarios and i'm going to show you how
to apply them hey man how's it going
your deployments failing Oh what are you
seeing crash oh okay let's have a look
so the first thing we're gonna do is we
say cube CTL get deployed so as I said
we look at the deployment first and we
can see here now we're gonna look at
statuses and events so the first thing
statuses we can see there's 0 radiate of
2 so we want to but for some reason the
pods are
not being created so what we do next as
we described so you always do a get
check the statuses describe check the
events so we describe and we look
through and we look at the event so we
can see deployment is trying to scale up
to two and we can also see that there's
a desired two pods but they're they're
both unavailable so now we hop two pods
so we say cube CT I'll get pods and we
can see crash Lu back off so this is a
crash loop basically application will be
constantly restarted because it's
crashing and normally this indicates 99%
of the time it's an application problem
so the developer needs to try and run
this container locally first make sure
the container runs locally and before
deploying it to kubernetes if it runs in
local it should run on kubernetes unless
your configs or something as in correct
what I like to do next whenever
something is in a crash Lubeck we do
cube CT I'll give pods we grab the pot
name and we immediately do keep CTR logs
on that pot and that'll tell us the logs
for the application a stack trace or
something and we can see here this is an
engine X container and we've got some
dodgy syntax in the engine X
configuration so this gives us an
indication to get what's wrong we need
to go back to engine X we need to go to
cut out config and then we can go ahead
and fix that and that will stop the
crash failing again what are you seeing
some image issue ok well let's have a
look so first thing I do again
deployments then pods
we get the deployments I can immediately
see again that there's something wrong
with these statuses we have zero out of
two ready so what we're gonna do next is
hop over to the describe command so
let's describe the deployment look at
the statuses and events we can see we're
desired again - we have 3 unavailable so
obviously kubernetes is trying to create
these containers over and over again we
can also see based on the events the
deployment is trying to scale - all
right and I'm gonna clear that out my
next step is to take a look at the pot
so we look take a look at the deployment
we know there's something wrong with the
pod
so we take a look at the pods and we see
error and image pulled back off so
interesting so image pullback Wolff
means it's basically backing off for the
deployment because the image cannot be
pulled so if we take this far is it here
CTL described pod you can describe both
deployments and pods and when we take a
look at this statuses first Rama status
and events we look at status we see
pending it cannot deploy why not we look
at the events pull image engine X
missing okay it's trying to pull an
engine X image with a tag that doesn't
exist it's telling us here that the
manifest for engine X missing is not
found so we have a docker container
image that doesn't exist in the registry
now there's two types of areas that can
happen here one is it's trying to pull
it from a private repository and the
developer has not defined the secret to
pull the image so double-check that if
you're pulling from private repos and
number two the image is just plain
missing so if you do docker pull locally
for nginx missing you'll get the same
error deployment work okay web traffic
is failing you're getting errors every
now and again
okay so there's instability and
deployment has worked so let's take a
look again deployment then part I'm
gonna do a cute CTL get deploy and we
can see we have all ready to again we
have the the deployment 0 ready
it looks like there's it's like every
now and again there is a pod that's up
I'm gonna do keep CT I'll describe
deploy zero unavailable they're all up
they're all running so we take a look at
that we can see that deployment is
continuously scaling up and down okay so
the next thing I'm gonna do is I'm gonna
take a look at the pods there's a crash
this one's running look at the restart
counts so when I say its statuses and
events we looked at the deployment we
could see that there were some
instabilities in the deployment with the
numbers of replicas it's trying to scale
up continuously
we do get pods we look at the status as
we can see we have a crashing one but we
have restart counters going up now
that's why at some point you will always
have running pods but because something
is happening they're continuously
crashing restart counters going up all
the time and this means that you have
instabilities and when these containers
are crashing you might have intermittent
network issues intimates and latency
spikes and things like that so let's
take a look at these pods cube CTL
described pod and we can see crash Leah
back off last data is terminated and we
take a look at the events we can see
liveliness probe failed status 404 so
this application is has a liveliness
probe defined to an end point that
possibly does not exist or the end point
is not healthy so I would always advise
if you see liveliness probe and
readiness Pro failing make sure you
configure it and set up these probes
correctly and also run your docker
container locally make sure you can hit
that readiness and liveliness Pro
running nice so you can send traffic to
it okay did you try rendered locally
okay so let's take a look so again cube
CTL get deployed we take a look at the
deployment we can notice again replicas
are not to out of tooth we're going to
describe deploy and we can again see
everything looks good except for that
all our pods are unavailable there's a
mound there's everything here and we can
see that it's trying to scale up okay so
it looks like the deployment is
struggling we say cube CTL to give pods
and we look at container creating so
sometimes containers can be stuck into
creating States because of various
reasons so if we take a look at the pod
we're gonna go cube CTL described pod
the scribe is your friend we can see
it's in a pending status and if we
scroll all the way down and look at the
events remember statuses and events we
can see it's trying to set up a mount
for config map example config which is
not found if I do cube CT I'll get
config map notice that there's no config
map so the developer in this case has
deployed a pod and without actually
deploying the config map and this can
happen with secrets as well you deploy
your app you have you forgot to deploy
the secret or the secret exist in one
environment or not in the other
environment and you'll have this issue
so this is a simple one make sure you
deploy your config maps and secrets and
redeploy your application
[Music]
it's stuck against what is the status oh
it's in a pending status well let's take
a look so again the first thing we do
cube CT I'll get deployed you notice
that we again have pods that are not
ready so the next thing we're gonna do
is describe that deployment we take a
look and we can immediately see we have
unavailable pods and we desire to we can
see we're trying to scale up so there's
a problem at the pod level that's
preventing this deployment from scaling
and meeting its desired state so
I'm going to do TM CTR to get pods and
we can see here it's trying to create
containers but they're stuck in a
pending state now why is that the only
way we'll know is if we describe the
pods so keeps ETL described pod and that
prints out everything against status and
is pending and events we look at events
and we can notice that nodes are not
available in sufficient memory now
there's a bunch of things that can cause
pending status like this one is you
might have a node selector where you say
I want to deploy this pod to specific
node that has specific labels on it that
can when that node is not available
you'll get a pending status for that the
other one is resources that you define
in your pod yeah moe
if you sound one X amount of memory X
amount of CPU and that condition cannot
be satisfied your pods will go into
pending status the other one is when the
scheduler has difficulty so let's say
your infrastructure is running hot
there's a lot of pods running on a
machine and there's not enough space to
put your pod because you have some
resources and elements that you want to
or request values and kubernetes will
not schedule that application will go
into pending mode because there's just
no place to reliably schedule that
application so normally it's a
scheduling prompt so in this case we
have insufficient memory now if we go
and we say cube CTL to get deploy we
take that deployment we say cube CTL get
deploy pop the name say - oh yeah mole
so let's do take a look at what's been
defined and when we take a look we can
see here we have to find some resource
limits and request values and we can see
the developer has made a typo here and
requests a lot of extra zeros and that
is basically trying to over schedule
that so all we need to do is remove
those zeros reapply the llamó file and
we'll be good so sometimes you know you
can make these kind of mistakes and
that's kind of what the pending status
means it's all about scheduling okay so
that should solve most of deploying
and pod related issues on kubernetes one
other little tip and trick I want to I
want to show you guys is about traffic
so when traffic hits the pod a lot of
the times the deployment is successful
but you can't get traffic to the
container so I want to show you guys I
have a deployment running and I have two
pods running from that deployment and
now we want to troubleshoot and see why
developer can't basically request it'll
get access to that to that pod from a
traffic point of view so what I normally
do is I describe the pods I say cube CTL
describe the pod and I noticed that the
ports that I exposed on the application
is port 80 now sometimes what happens is
when developer writes code is they might
say the application is exposing port
5,000 or 30,000 or some arbitrary port
on that local machine but then the
container or kubernetes is actually
listening or looking for port 80 so
there's port mismatches so always make
sure you run your docker container
locally first and when you do the docker
run command and you specify the ports
you make sure that you map the port that
you're expecting in the browser to match
the port of the container now when you
describe the pod and you get that port
number you can use a feature called port
forward to test that so we can set port
forward and then the name of the pod and
then we're gonna put forward support
eight and then if I open up the browser
and I hit that application or use curl
or postman or some kind of a traffic
generator I can see that okay I can hit
that pot on the right port so that's a
good way to confirm that your deployment
is exposing the right port to the
container okay so your deployment is
good you can same traffic to it did you
you tried port forwarding you make sure
your pod and containers actually
listening on the right port okay so now
you you can't your other micro service
Concord okay so this is how we look at
specifically troubleshooting kubernetes
service
and we've looked at pods and deployments
so now the developer is exposing his
application through a kubernetes service
to other micro services or to an ingress
controller and we know that the
container is listening on the right port
we can access it through port forward so
the next step is go up to the service
level so we see we have an example
service we list out nothing looks
obvious here we can see it's listening
on port 80 we know the nginx or the
micro service listening on port 80 as
well so the next thing I'm gonna do is
cube CTL described service with that
service name remember gates and
subscribe and describe or your most
biggest friends so we look here and
immediately I can see endpoints is blank
now when you deploy a service service
users labels to select the application
so if we take a look at the cube CTL get
deploy we can also say show labels we
see we have labels called app example
app on the deployment but when we
describe the service we see a selector
example app mistake so the developer
made a mistake by selecting putting the
wrong selector in sometimes you can put
the wrong selector on the service or you
can put the wrong labels on the
deployment that they both have to match
in order for the service to create the
relevant endpoints are you getting
connection refused from the other micro
servers so the next way to troubleshoot
a service now I can go do again the
describe and things like that but I want
to show you how to actually troubleshoot
a service using like curl and actually
making the request to see if that
service works so I'm gonna do keep CTR
gate pods and I'm going to jump into one
of these pots so I'm gonna say keep CTL
exec - IT pod name and I'm just gonna go
in
side of that one now you can go into any
container inside that namespace and try
maker a quest so I'm gonna make a
request to our example service and I
noticed connection refuses trying to
call on the service on port 80 and we
know that port 80 has been used on the
service and we know that the container
is listening on 80 as well so this means
this service could potentially be
misconfigured because there's no port 80
being listened on so that is one way you
can jump into pods use curl to test that
yourself but what I'm going to do is I'm
gonna say cube CTL describe service and
when I take a look at that I look at the
endpoints and I noticed that there's the
wrong ports so when we say cube CTL get
service and we pass in oh yeah Mille we
can get out the mo of definition of that
service and we notice that we're
defining port 80 but the target port is
wrong it's 5000 so these are some of the
alarm bells when it comes to services
you have to make sure your selector is
corrective to make sure your port flows
all the way from the exposed port to the
target port the deployment has the right
port that maps to the container port and
that if you fix that you'll be able to
make sure that traffic gets into your
container through the community service
right so that should help you create the
correct alarm bells to troubleshoot
deployments and services on kubernetes
so remember deployments first then pods
look at statuses then events if you have
a crash loop look at logs and for
services make sure selectors and port
mappings are defined correctly even on
the deployment and container level also
I encourage to always make sure your
docker containers run locally with the
docker run command before you actually
go on to kubernetes and hopefully that
helps you guys hopefully you enjoyed the
video like and subscribe and let me know
down in the comments what sort of stuff
you'd like to see in the future until
next time
peace


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwNzQ4MjYyNCwtMTIzNzI3MTAwNSwtOT
Y4ODY5MzUxXX0=
-->