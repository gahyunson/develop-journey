# Digest 
- The mechanism that compares the user's response with the data stored on the server.
- Focused on providing an authentication system that can't be easily manipulated.
- Encoding != Encrypting
- Encryption requires a key to decrypt.
- If someone doesn't have the key to an encrypted message, the message cannot be decrypted.

### Security Considerations
- 보호 수준 정보 저장 장소: WWW-Authenticate, Authorization Header
- 어떤 트랜잭션에서 엿들은 인증 자격을 다른 트랜잭션에 재전송 공격 가능
    - IP address, 짧은 타임아웃 값 조합으로 hack이 조금 어렵게 할 수있다.
- 클라이언트는 가장 강력한 인증 제도를 선택하는 것이 좋으나, 모든 클라이언트가 우리가 선택한 강력한 인증 제도를 실행할 수 있어야 한다. 사내 네트워크가 성공적인 예이다. 
- 다음 조건을 만족할 때, 비밀번호를 쉽게 수집할 수 있다. 1. 비밀번호 만료 정책이 없다. 2. 충분한 시간이 있다. 3. 비밀번호를크래킹할 1회 비용을 지불 할 수 있다.
    - 크래킹하기 어렵도록 상대적으로 복잡한 비밀번호를 사용한다.
    - 괜찮은 비밀번호 만료 정책
- 신뢰되는 프락시의 경우 프락시 자신의 확장 인터페이스에 의해서 이다. 
    - 클라이언트가 사용자에게 인증의 강도를 시각적으로 보여주기
    - 클라이언트가 언제나 가장 강력한 인증 선택
    - 유일한 실패하지 않는 방법: SSL 사용

- Location for storing security leve information: WWW-Authenticate, Authorization Header.
- Replay attack can occur by using credentials from one transaction in another transaction.
    - Set IP address hardly and short timeout.
- It is recommended that the client select the strongest authentication method, but the client must also be capable of running that method. A successful example is within an internal network.
- If the following conditions are met, passwords can be easily hacked: 1. No password expiration policy 2. Enough time 3. Willingness to invest a one-time cost in password cracking
    - Set complex a password to make cracking more difficult.
    - implement a good password expiration policy.
- For trusted proxies, this is managed through the proxy's extension interface.
    - The client should visually display the strength of the authentication to the user.
    - The client should always select the strongest authentication method.
    - The only way not to fail: Use SSL
- Password file exposure
    - Keep the file secure.
    - Use unique naming to localize damage.

- slightly slower than Basic.
- vulnerable to a man-in-the-middle security attack
- password stored on the server could be hacked

If the client could pass the nonce of the next before the server asks,
1. A client --(a request)--> A server
2. The server --(a special code) + (asks to authenticate)--> The client
3. The client --(an encrypted version of info)--> The server
4. The server --(the requested info) or (error)--> The client

### Digest And Basic
**Digest**: An encrypted form by applying a hash function.    
**Basic**: non-encrypted base64 encoding. should be used where transport layer security is provided such as https.


Reference [[1]]('https://stackoverflow.com/questions/9534602/what-is-the-difference-between-digest-and-basic-authentication') [[2]]('https://en.wikipedia.org/wiki/Digest_access_authentication') [[3]]('https://www.amazon.co.uk/HTTP-Definitive-Guide-David-Gourley/dp/1565925092')
