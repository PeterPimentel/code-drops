# Segurança de aplicações

[Play by Play: JavaScript Security](https://app.pluralsight.com/courses/6269a7f2-617f-47ca-8405-9842eb078e02/table-of-contents)

[Practical Cryptography in Node.js](https://app.pluralsight.com/library/courses/practical-cryptography-node-js/table-of-contents)

### Dicas
- Versão do script com CDN

Quando usar CDN ir buscar diretamente a versão utilizada no desenvolvimento.
Além da buscar ser mais rápida previne a chamada a um script com vulnerabilidade.

- Utilizar CORS a sey favor

Informar de onde as requisições virão não permitir requisições de todas as origens.

- Artigo falando sobre a inserção de scripts maliciosos em pacotes npm

[https://medium.com/hackernoon/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5](https://medium.com/hackernoon/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5)

- Linter para websites

Linter para websites mas que verifica as dependências, e outras coisas.
Ele faz request para as aplicações rodando e realiza verificações.
Regras de segurança por exemplo scripts com vulnerabilidades.
[https://webhint.io/](https://webhint.io/)

 Tipos de criptografia usando crypto
- MD5

 ``crypto.createHash('md5')``
 Já foram descobertas várias vulnerabilidades nesse tipo de criptografia
Site que mostra a descriptografia do hash - [hashtoolkit](https://hashtoolkit.com/)

- SHA-256

 ``crypto.createHash('sha256')``
Hash muito maior que o md5, porém ainda inseguro


### Good Hash Algorithms
Seguindo a ordem de recomendação do OWASP

1. Argon2 - Best choice para uso acadêmico
2. PBKDF2 - é necessário um grande processo computacional para quebrar por força
3. scrypt - otimizado para resistir a based ataque hardwares
4. bcrypt


### Salt

Salt é um valor aleatório introduzido ao gerar o hash para dificultar a quebra.
Deve ser gerado um novo salt a cada login para cada usuário.

Exemplo: se dois usuários usam o mesmo algoritmo e a mesma senha, sem o salta as senhas cadastradas no banco seriam iguais, mas usando o salta elas se tornam diferentes.

The reason we use salts is to stop precomputation attacks, such as [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table). These attacks involve creating a database of hashes and their plaintexts, so that hashes can be searched for and immediately reversed into plaintext.

Excelente [explicação](https://security.stackexchange.com/questions/17421/how-to-store-salt) sobre o uso de salt keys e como armazená-las.

O salt pode ser armazenado no banco junto com a senha.

#### Como gerar o salt
``const crypto = require('crypto')``

```const salt = crypto.randomBytes(256).toString('hex')```

### Criando o hash

O ideal é usar a versão assíncrona pois o processo de criptografia demora de acordo com o número de interações passado.

``crypto.pbkdf2(password1, salt, 100000, 512, 'sha512', callback)``

 1. Senha a ser encriptada
 2. O Salt key gerado
 3. Número de interações
 4. Quantos bytes serão retornados da função
 5. A função de hash que será usada
 6. Função de callback recebendo ``(err, hashedPwd ) => {}``

O resultado é um buffer

```hashedPwd.toString('hex')```

### Como comprar o hash