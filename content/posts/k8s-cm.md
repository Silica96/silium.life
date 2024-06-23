+++
title = 'k8s config map 업데이트 후  적용시간'
date = 2024-06-23T20:27:34+09:00
draft = false
+++


컨피그맵을 deployment 에 마운트 하여 사용하는 설정에서 
컨피그맵 업데이트 후 적용되는데에 실제 어느정도 걸리는지 먼저 테스트를 진행해보았습니다.


테스트 진행 결과
- 1차 테스트 
	- 39:03 컨피그맵 업데이트 진행 
	- 39:35 업데이트 완료 관측
- 2차 테스트
	- 41:50 업데이트 진행
	- 42:10 완료
- 3차 테스트
	- 42:50 업데이트 진행 
	- 43:21 완료

3번의 실험 결과 약 30초 후 적용 되는것 확인 되었습니다.



테스트를 해보았을떄 실제 특정 시간을 가지고 업데이트 된다는것이 확실시되어 실제 업데이트 시간이 더 궁금해졌습니다.

때문에 쿠버네티스 공식문서를 찾아보았을때 다음과 같은 내용을 확인할 수 있었습니다.


> Kubelet checks whether the mounted ConfigMap is fresh on every periodic sync. However, it uses its local TTL-based cache for getting the current value of the ConfigMap. As a result, the total delay from the moment when the ConfigMap is updated to the moment when new keys are projected to the pod can be as long as kubelet sync period (1 minute by default) + TTL of ConfigMaps cache (1 minute by default) in kubelet. You can trigger an immediate refresh by updating one of the pod's annotations.


즉 kubelet에서 kubelet 동기화 기간(기본 1분) + 컨피그맵 캐시의 TTL(기본 1분)으로 
기본값 변경이 없다는 가정하에 최대 2분이 걸린다는것을 확인 할 수 있습니다.


또한 subPath 를 이용하여 컨피그 맵을 마운트한 경우 자동 업데이트가 적용되지 않는 한계점이 있습니다.

> A container using a ConfigMap as a subPath volume will not receive ConfigMap updates.

- 참고문서 : https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#mounted-configmaps-are-updated-automatically
