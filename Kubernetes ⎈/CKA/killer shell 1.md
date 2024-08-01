
1번 
You have access to multiple clusters from your main terminal through `kubectl` contexts. Write all those context names into `/opt/course/1/contexts`.

Next write a command to display the current context into `/opt/course/1/context_default_kubectl.sh`, the command should use `kubectl`.

Finally write a second command doing the same thing into `/opt/course/1/context_default_no_kubectl.sh`, but without the use of `kubectl`.


2번
Use context: `kubectl config use-context k8s-c1-H`

Create a single _Pod_ of image `httpd:2.4.41-alpine` in _Namespace_ `default`. The _Pod_ should be named `pod1` and the container should be named `pod1-container`. This _Pod_ should **only** be scheduled on controlplane nodes. Do not add new labels to any nodes.


3번
Use context: `kubectl config use-context k8s-c1-H`

There are two _Pods_ named `o3db-*` in _Namespace_ `project-c13`. C13 management asked you to scale the _Pods_ down to one replica to save resources.


4번
Use context: `kubectl config use-context k8s-c1-H`

Do the following in _Namespace_ `default`. Create a single _Pod_ named `ready-if-service-ready` of image `nginx:1.16.1-alpine`. Configure a LivenessProbe which simply executes command `true`. Also configure a ReadinessProbe which does check if the url `http://service-am-i-ready:80` is reachable, you can use `wget -T2 -O- http://service-am-i-ready:80` for this. Start the _Pod_ and confirm it isn't ready because of the ReadinessProbe.

Create a second _Pod_ named `am-i-ready` of image `nginx:1.16.1-alpine` with label `id: cross-server-ready`. The already existing _Service_ `service-am-i-ready` should now have that second _Pod_ as endpoint.

Now the first _Pod_ should be in ready state, confirm that.

