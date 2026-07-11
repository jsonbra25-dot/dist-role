# DIST. ROLE

App de consulta de preços da Role Distribuidora e Conveniência.

## O que o app faz
- Cadastro de produtos com preço unitário e preço por caixa
- Abas por categoria (Cervejas, Gin, Whisky, etc.)
- Busca rápida por nome
- Catálogo compartilhado em tempo real entre os dois celulares (via Firebase)

## Configuração necessária (antes de publicar)
Este app usa o **Firebase Firestore** como banco de dados compartilhado. Antes de publicar,
é preciso colar a configuração do seu projeto Firebase no arquivo `index.html`.

1. Acesse https://console.firebase.google.com e crie um projeto
2. Vá em **Firestore Database → Criar banco de dados → modo de teste**
3. Vá em **Configurações do projeto → Seus apps → Adicionar app Web (`</>`)**
4. Copie o bloco `firebaseConfig` gerado
5. Abra o `index.html`, procure por:
   ```js
   const firebaseConfig = {
     apiKey: "COLE_AQUI",
     ...
   };
   ```
   e substitua pelos valores copiados

## Publicando com GitHub + Vercel

1. Crie um repositório novo no GitHub (ex: `dist-role`)
2. Suba este arquivo `index.html` (e este `README.md`) para o repositório
3. Acesse https://vercel.com, faça login com sua conta GitHub
4. Clique em **Add New → Project**, selecione o repositório `dist-role`
5. Deixe as configurações padrão (é um site estático, sem build necessário) e clique em **Deploy**
6. O Vercel vai gerar um link público, algo como `dist-role.vercel.app`

## Atualizando o app no futuro
Sempre que quiser mudar algo (nova cor, novo campo, etc.):
1. Suba o novo `index.html` no GitHub (substituindo o antigo, ou via commit)
2. O Vercel detecta o novo commit e republica automaticamente — nenhum passo extra necessário

## Acesso administrador (novo)
Agora o app funciona em dois modos:
- **Modo público (padrão):** qualquer pessoa que abrir o link só consulta e busca produtos — sem botão de adicionar, editar ou excluir. Ideal para compartilhar com clientes.
- **Modo administrador:** toque no ícone de cadeado 🔒 no canto do cabeçalho. Na primeira vez, o app pede para **criar uma senha de administrador** (mínimo 4 caracteres) — essa senha fica salva de forma protegida (hash) no Firebase. Da segunda vez em diante, é só digitar a senha para entrar.

Enquanto estiver no modo administrador (badge amarelo "ADMIN" visível), aparecem os botões de adicionar produto, editar, excluir e gerenciar abas. Para trocar a senha, abra **Gerenciar abas** e use o campo "Senha de administrador" no final. Para sair do modo admin, toque no badge "ADMIN" novamente.

O login fica salvo no navegador do celular (não precisa digitar a senha toda vez no mesmo aparelho), mas pode ser desfeito a qualquer momento tocando em "Sair".

## Segurança do Firestore

O modo de teste do Firestore libera leitura/escrita por 30 dias. Depois disso, ajuste as regras
em **Firestore Database → Regras** para algo como:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /role-catalog/{doc} {
      allow read, write: if true;
    }
  }
}
```
Isso mantém o catálogo aberto apenas para quem tiver o link do app — suficiente para um uso interno
de dois usuários, mas não indicado se o link for compartilhado publicamente.
