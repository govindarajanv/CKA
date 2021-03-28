- netstat -anp and netstat -nltpu
- if pvc is not bound despite matching access mode and storage class, please check if sc's VolumeBindingMode is set to WaitForFirstConsumer
- Please refer role and rolebinding doc for validating the config. Also "resourceName: should have been correctly configured. Correct namespace should be used
- Annotations are important for ingress

