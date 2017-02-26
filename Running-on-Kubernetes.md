The matter was discussed in an issue. Many thanks to @styxlab who did share this configuration : 

```yaml
apiVerapiVersion: v1
kind: Service
metadata:
  name: redis
  namespace: default
  labels:
    app: mailu
    role: mail
    tier: backend
spec:
  selector:
    app: mailu
    role: mail
    tier: backend
  ports:    
  - name: redis
    port: 6379
    protocol: TCP

---

apiVersion: v1
kind: Service
metadata:
  name: antispam
  namespace: default
  labels:
    app: mailu
    role: mail
    tier: backend
spec:
  selector:
    app: mailu
    role: mail
    tier: backend
  ports:    
  - name: antispam
    port: 11333
    protocol: TCP

---

apiVersion: v1
kind: Service
metadata:
  name: antivirus
  namespace: default
  labels:
    app: mailu
    role: mail
    tier: backend
spec:
  selector:
    app: mailu
    role: mail
    tier: backend
  ports:    
  - name: antivirus
    port: 3310
    protocol: TCP

---

apiVersion: v1
kind: Service
metadata:
  name: imap
  namespace: default
  labels:
    app: mailu
    role: mail
    tier: backend
spec:
  selector:
    app: mailu
    role: mail
    tier: backend
  ports:
  - name: imap-auth
    port: 2102
    protocol: TCP
  - name: imap-transport
    port: 2525
    protocol: TCP

---

apiVersion: v1
kind: Service
metadata:
  name: mailu
  namespace: default
  labels:
    app: mailu
    role: mail
    tier: backend
spec:
  selector:
    app: mailu
    role: mail
    tier: backend
  ports:
  - name: http
    port: 80
    protocol: TCP
  - name: imap-default
    port: 143
    protocol: TCP
  - name: imap-ssl
    port: 993
    protocol: TCP
  - name: sieve
    port: 4190
    protocol: TCP
  - name: smtp
    port: 25
    protocol: TCP
  - name: smtp-ssl
    port: 465
    protocol: TCP
  - name: smtp-starttls
    port: 587
    protocol: TCP

---

apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: mailu
  namespace: default
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mailu
        role: mail
        tier: backend
    spec:
      containers:
      - name: admin
        image: mailu/admin:stable
        imagePullPolicy: Always
        env:
          - name  : DOMAIN
            value : example.com
          - name  : HOSTNAME
            value : mail.example.com
          - name  : POSTMASTER
            value : admin
          - name  : SECRET_KEY
            value : ChangeMeChangeMe
          - name  : DEBUG
            value : "True"
        volumeMounts:
          - name: maildata
            mountPath: /data
          - name: dkim
            mountPath: /dkim
          - name: certs
            mountPath: /certs
            readOnly: true
#          - name: docker
#            mountPath: /var/run/docker.sock
#            readOnly: true
        ports:
          - name: http
            containerPort: 80
            protocol: TCP
      - name: redis
        image: redis:latest
        imagePullPolicy: Always
        volumeMounts:
          - mountPath: /data
            name: redisdata
        ports:
          - containerPort: 6379
            name: redis
            protocol: TCP
      - name: imap
        image: mailu/dovecot:stable
        imagePullPolicy: Always
        env:
          - name  : DOMAIN
            value : example.com
          - name  : HOSTNAME
            value : mail.example.com
          - name  : POSTMASTER
            value : admin
        volumeMounts:
          - mountPath: /data
            name: maildata
          - mountPath: /mail
            name: mailstate
          - mountPath: /overrides
            name: overrides
          - mountPath: /certs
            name: certs
            readOnly: true
        ports:
          - containerPort: 2102
            name: imap-auth
            protocol: TCP
          - containerPort: 2525
            name: imap-transport
            protocol: TCP       
          - containerPort: 143
            name: imap-default
            protocol: TCP          
          - containerPort: 993
            name: imap-ssl
            protocol: TCP
          - containerPort: 4190
            name: sieve
            protocol: TCP
      - name: smtp
        image: mailu/postfix:stable
        imagePullPolicy: Always
        env:
          - name  : DOMAIN
            value : example.com
          - name  : HOSTNAME
            value : mail.example.com
          - name  : MESSAGE_SIZE_LIMIT
            value : "50000000"
          - name  : RELAYHOST
            value : ""
        volumeMounts:
          - mountPath: /data
            name: maildata
          - mountPath: /overrides
            name: overrides
          - mountPath: /certs
            name: certs
            readOnly: true
        ports:
          - name: smtp
            containerPort: 25
            protocol: TCP
          - name: smtp-ssl
            containerPort: 465
            protocol: TCP
          - name: smtp-starttls
            containerPort: 587
            protocol: TCP
      - name: milter
        image: mailu/rmilter:stable
        imagePullPolicy: Always
        ports:
          - name: milter
            containerPort: 9900
            protocol: TCP
        volumeMounts:
          - name: maildata
            mountPath: /data
          - name: dkim
            mountPath: /dkim
          - name: overrides
            mountPath: /overrides
          - name: certs
            mountPath: /certs
            readOnly: true
      - name: antispam
        image: mailu/rspamd:stable
        imagePullPolicy: Always
        ports:
          - name: antispam
            containerPort: 11333
            protocol: TCP          
        volumeMounts:
          - name: filter
            mountPath: /var/lib/rspamd
      - name: antivirus
        image: mailu/clamav:stable
        imagePullPolicy: Always
        ports:
          - name: antivirus
            containerPort: 3310
            protocol: TCP
        volumeMounts:
          - name: filter
            mountPath: /data
      volumes:
        - name: redisdata
          emptyDir: {}        
        - name: maildata
          emptyDir: {}
        - name: mailstate
          emptyDir: {}
        - name: overrides
          emptyDir: {}
        - name: dkim
          emptyDir: {}       
        - name: filter
          emptyDir: {}   
        - name: certs
          secret:
            items:
              - key: tls.crt
                path: cert.pem
              - key: tls.key
                path: key.pem
            secretName: mail-example-com-letsencrypt-ssl
#        - name: docker
#          hostPath:
#            path: /var/run/docker.sock
      imagePullSecrets:
        - name: myregistrykey
```