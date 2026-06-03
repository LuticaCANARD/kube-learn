# Kubernetes Local Troubleshooting Manual

## 실습 문서 규칙

어떠한 종류의 실습이든 종료 후에는 Minikube 안에 생성한 리소스를 정리한다.

공통 정리 절차는 [docs/cleanup-minikube.md](docs/cleanup-minikube.md)에 기록한다.

모든 실습 문서의 마지막에는 `실습 종료 후 정리` 항목을 포함한다.

## kubectl get pods 연결 거부 오류

### 증상

`kubectl get pods` 실행 시 아래와 비슷한 오류가 반복된다.

```powershell
Unable to connect to the server: dial tcp 127.0.0.1:40659: connectex: No connection could be made because the target machine actively refused it.
```

또는 다음 로그가 함께 출력된다.

```powershell
couldn't get current server API group list: Get "https://127.0.0.1:40659/api?timeout=32s"
```

### 원인

현재 `kubectl` 컨텍스트가 `minikube`를 가리키고 있지만, Minikube 클러스터가 중지되어 Kubernetes API 서버가 실행 중이지 않은 상태다.

즉, `kubectl`은 아래와 같은 로컬 API 서버 주소로 접속하려 하지만,

```text
https://127.0.0.1:40659
```

해당 포트에서 API 서버가 떠 있지 않아 연결이 거부된다.

### 확인 절차

현재 `kubectl` 컨텍스트를 확인한다.

```powershell
kubectl config current-context
```

예상 출력:

```text
minikube
```

활성 컨텍스트와 클러스터 정보를 확인한다.

```powershell
kubectl config get-contexts
kubectl config view --minify
```

Minikube 상태를 확인한다.

```powershell
minikube status
```

문제 상황에서는 보통 아래처럼 표시된다.

```text
host: Stopped
kubelet: Stopped
apiserver: Stopped
kubeconfig: Stopped
```

### 복구 절차

Minikube를 시작한다.

```powershell
minikube start
```

시작 완료 후 상태를 다시 확인한다.

```powershell
minikube status
```

정상 상태:

```text
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

`kubectl` 연결을 검증한다.

```powershell
kubectl cluster-info
kubectl get pods
```

정상 예시:

```text
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   1          5d1h
```

### 빠른 복구 명령

같은 오류가 다시 발생하면 아래 순서대로 실행한다.

```powershell
minikube status
minikube start
kubectl get pods
```

### 참고

`minikube start` 실행 중 아래와 같은 경고가 나올 수 있다.

```text
Error downloading kic artifacts: not yet implemented
```

이번 사례에서는 Minikube가 정상 시작되었고 `kubectl get pods`도 성공했으므로, 이 경고만으로는 실패로 판단하지 않는다. 최종 판단은 `minikube status`와 `kubectl get pods` 결과로 한다.
