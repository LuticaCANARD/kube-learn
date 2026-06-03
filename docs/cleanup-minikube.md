# Minikube 실습 종료 후 초기화

## 목적

어떠한 종류의 Kubernetes 실습이든 종료 후에는 Minikube 안에 생성한 리소스를 정리한다.

이 절차를 매번 수행하면 다음 실습에서 이전 Pod, Service, Deployment, ConfigMap, Secret, PVC 등이 영향을 주는 상황을 줄일 수 있다.

## 기본 정리 명령

일반 실습 종료 후에는 먼저 현재 네임스페이스의 주요 리소스를 삭제한다.

```powershell
kubectl delete all --all
kubectl delete configmap --all
kubectl delete secret --all
kubectl delete pvc --all
kubectl delete ingress --all
```

기본 네임스페이스가 아닌 다른 네임스페이스에서 실습했다면 `-n` 옵션을 붙인다.

```powershell
kubectl delete all --all -n <namespace>
kubectl delete configmap --all -n <namespace>
kubectl delete secret --all -n <namespace>
kubectl delete pvc --all -n <namespace>
kubectl delete ingress --all -n <namespace>
```

## 정리 확인

아래 명령으로 남은 리소스를 확인한다.

```powershell
kubectl get all
kubectl get configmap
kubectl get secret
kubectl get pvc
kubectl get ingress
```

`default` 네임스페이스 기준으로 실습 리소스가 남아 있지 않으면 정리 완료로 본다.

## 전체 네임스페이스 확인

실습 중 네임스페이스를 따로 만들었다면 전체 네임스페이스의 리소스를 확인한다.

```powershell
kubectl get all --all-namespaces
kubectl get pvc --all-namespaces
kubectl get ingress --all-namespaces
```

실습용 네임스페이스 자체를 삭제하려면 다음 명령을 사용한다.

```powershell
kubectl delete namespace <namespace>
```

## 완전 초기화가 필요할 때

Minikube 클러스터 자체를 완전히 지우고 새로 시작해야 할 때만 아래 명령을 사용한다.

```powershell
minikube delete
minikube start
```

이 명령은 Minikube 클러스터를 삭제하므로, 실습 리소스뿐 아니라 클러스터 내부 상태도 함께 사라진다.

## 실습 문서 작성 규칙

앞으로 모든 실습 문서의 마지막에는 아래 항목을 포함한다.

    ## 실습 종료 후 정리

    ```powershell
    kubectl delete all --all
    kubectl delete configmap --all
    kubectl delete secret --all
    kubectl delete pvc --all
    kubectl delete ingress --all
    ```
