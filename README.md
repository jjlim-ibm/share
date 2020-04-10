< 로그 확인>
API Manager
- VM #1에 ssh 로그인
- APIM pod 이름 확인
  kubectl get po |grep apim-v2
- pod에서 생성하는 로그 확인
  kubectl logs <apim pod 명> : 3개가 나오면 각각 로그를 봐야함.
    예) kubectl logs <apim> -f : tail -f 와 유사
        kubectl logs <apim> --since=5m -f : 전체 로그가 아니라 5분 전부터의 tail
        kubectl logs <apim> > apim.log : apim.log로 전체 로그를 받아서 확인
Gateway
- Workstation 로그인
- cloudctl -a https://<icp cluster명>:8443
- apim-master/1234 로 로그인. namespace도 gateway가 있는 namespace로 지정 (hcp-apim?)
- gateway pod 로그 확인 (위와 동일) - 이 로그는 system log (default-log)이므로 별 내용이 없을 공산이 크므로, gwd.log 확인이 필요함
- gateway pod 로그 확인 (gwd)
  kubectl exec -it <gateway pod> -- sh : pod 내부로 진입
  cd /drouter/temporary/log/apiconnect
  여기서 gwd-log.* 의 내용 확인 필요
  
  혹은 workstation으로 로그를 copy 하여 내려받아서 봐도 무방함
  kubectl cp <gateway pod>://drouter/temporary/log/apiconnect/gwd-log.log gwd-log.log
  kubectl cp <gateway pod>://drouter/temporary/log/apiconnect/gwd-log.log.1 gwd-log.log.1
  kubectl cp <gateway pod>://drouter/temporary/log/apiconnect/gwd-log.log.2 gwd-log.log.2
  kubectl cp <gateway pod>://drouter/temporary/log/apiconnect/gwd-log.log.3 gwd-log.log.3

<로그 확인 내용>
- create subscription 시에 APIM이나 gwd 로그에 에러가 있는가?
- 혹은 그냥 열어봐도 APIM 이나 gwd 로그에 에러가 있는가? 있다면 어떤게 있는가? 지금 발생하는 이슈와 로그가 관계가 있어 보이는가?
  kubectl logs <apim> |grep -i "bhendi:error"
  kubectl logs <apim> |grep -i "apim:error"
  
참고로 로그 내용이 워낙 많으므로 주의하셔야 합니다. (로그가 덮어써질 수 있고, 3대라서 확인이 어려움)
