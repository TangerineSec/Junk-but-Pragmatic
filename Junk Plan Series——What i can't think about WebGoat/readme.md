# About what I can't know.

## A1.1：Hijack a session

**Concept**

Applicayion developers who their own session IDs frequently forget to incorporate the complexity and randomness necessrary for security.If the user specific session ID is not complex and random,then the application is highly susceptible to session-based brute force attacks.

**Goals**

Gain access to an authenticated session belonging to someone else.

In this lesson we are tring to predict the 'hijack_cookie'.The 'hijack_cookie' is used to differentiate and anonymous users of WebGoat.

### 我能知道的？

![image-20240124151839848](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124151839848.png)

![image-20240124153223895](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124153223895.png)

![image-20240124155523039](https://githubwiki.oss-cn-shanghai.aliyuncs.com/img/typroa/image-20240124155523039.png)

### Solved Scheme



## A1.2：Insecure Direct Object References



## A1.3：Missing Function Level Access Control



## A1.4 Spoofing an Authentication