# Result of Deprecated Pod create / apply order

## On `kubectl apply -f ./test\deprecate-meta\pod-b.yaml -f .\test\deprecate-meta\pod-a.yaml`

- Mutated 2 times via b -> a.

## On `kubectl create -f ./test\deprecate-meta\pod-a.yaml -f ./test\deprecate-meta\pod-b.yaml`

- They rejected by `Error from server (AlreadyExists): error when creating "./test\\deprecate-meta\\pod-b.yaml": pods "app" already exists`.
- Blocking by lock about new pods.
