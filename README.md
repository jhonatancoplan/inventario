# inventario

## 1. Introdução

### a. Qual o objetivo da atividade?
O objetivo deste projeto é criar um sistema de inventário de itens de um jogo de sua escolha. O sistema permitirá que o usuário faça login, acesse seu inventário e cadastre itens. O inventário exibirá os itens cadastrados para o usuário.

**O que é um inventário em um jogo? Qual a finalidade? Dê exemplos.**  
Um inventário em um jogo é uma área onde o jogador pode armazenar itens coletados durante a partida. Ele permite que o jogador acesse e use esses itens conforme necessário. Exemplos de jogos que utilizam inventários incluem **The Legend of Zelda**, e **Undertale**.

**Que tipos de sistemas utilizam essa funcionalidade? Dê exemplos.**  
Essa funcionalidade de inventário pode ser encontrada em **jogos eletrônicos**, **aplicações de gerenciamento de recursos** (como softwares de estoque e gestão) e até em **aplicativos de compras** online, onde você armazena produtos que deseja comprar. Outros exemplos incluem **Mercado Livre** ou **Minecraft**.

**Porque essa funcionalidade é importante?**  
O inventário é importante porque ajuda o jogador a gerenciar os itens que ele adquire, essencial para a progressão no jogo. Também permite que o jogador organize seu espaço virtual, facilitando o uso e troca de itens durante a jornada.

## 2. A Implementação

### a. Front-end

**i. Quais ferramentas foram utilizadas (editores/linguagens)? Porque? O que cada um deles faz?**  
O sistema foi desenvolvido usando as seguintes ferramentas:

- **HTML:** Para estruturar o conteúdo das páginas do site, como o formulário de cadastro e a exibição dos itens do inventário.
- **CSS:** Para estilizar as páginas, deixando o visual mais atraente e organizado. Utilizamos o Bootstrap para facilitar o design responsivo.
- **JavaScript:** Para implementar a interatividade, como mostrar ou esconder o campo para adicionar a imagem, quando a caixa de seleção for marcada.

**ii. Como o layout foi definido? Como a interface foi setorizada? Relação linhas x colunas.**  

O layout do sistema foi definido utilizando o **framework Bootstrap**

A interface foi dividida em áreas específicas, como:

- **Cabeçalho (Navbar):** Contém links de navegação, como "Voltar" e "Sair".
- **Formulário de Cadastro:** O usuário pode inserir o nome do item, quantidade e uma opção para adicionar uma imagem.
- **Tabela de Itens:** Mostra todos os itens cadastrados no inventário, com nome, quantidade e imagem, se fornecida.

A interface é responsiva, ou seja, ela se adapta ao tamanho da tela (seja em desktop ou dispositivo móvel), aproveitando o sistema de grid do Bootstrap.

Relação linhas x colunas é 1x3 

### b. Back-end

**i. Quais ferramentas foram utilizadas (editores/linguagens)? Porque? O que cada um deles faz?**  
No back-end, o projeto utiliza **PHP** para gerenciar o login do usuário, o cadastro de itens no inventário e a persistência dos dados. 

**ii. Sobre o código PHP**

**1. O que o código faz? Explicar as principais funcionalidades com exemplos de código.**

- **Cadastro de itens:** O código PHP verifica se o usuário está logado e, caso esteja, ele permite que o item seja cadastrado. O código também salva as informações dos itens em um arquivo de texto (`inventario.txt`). 
```php
if (isset($_POST['nome']) && isset($_POST['quantidade'])) {
    $nome = trim($_POST['nome']);
    $quantidade = trim($_POST['quantidade']);
    $imagem = ""; // Inicializando a variável da imagem

    // Verifica se a opção de imagem foi marcada
    if (isset($_POST['imagem-opcao']) && $_POST['imagem-opcao'] == 'on') {
        if (isset($_POST['imagem-url']) && !empty($_POST['imagem-url'])) {
            $imagem = trim($_POST['imagem-url']);
        }
    }

    // Validação e salvamento dos dados
    if (!empty($nome) && is_numeric($quantidade) && $quantidade > 0) {
        $arquivo = fopen("../inventario.txt", "a");
        fwrite($arquivo, "$nome|$quantidade|$imagem\n"); 
        fclose($arquivo);
    }
}
```

- **Login de usuários:** O código PHP também gerencia a autenticação dos usuários, comparando o nome de usuário e a senha inseridos com as credenciais definidas no código.
```php
if ($_POST['usuario'] === USUARIO && $_POST['senha'] === SENHA) {
    $_SESSION['logado'] = true;
    header("Location: ../index.php");
} else {
    echo "<script>alert('Usuário ou senha inválidos!'); window.location.href='../login.php';</script>";
}
```

## 3. Passo a passo de execução

### a. Como executar o projeto

Para rodar o projeto localmente, siga os passos abaixo:

1. Baixe ou clone o repositório do GitHub para sua máquina.
2. Instale um servidor local, como o **XAMPP** ou **WAMP**.
3. Coloque o projeto dentro da pasta `htdocs` (se estiver usando o XAMPP) ou a pasta correspondente ao seu servidor local.
4. Acesse a URL do projeto no seu navegador (por exemplo, `http://localhost/inventario`).
5. O sistema pedirá um login, então use as credenciais definidas no código PHP para acessar o inventário.

### b. Hierarquia de diretórios

A estrutura de diretórios do projeto é a seguinte:

```
/
├── /pages              # Contém as páginas principais como cadastro, inventário e login
│   ├── cadastro.php    # Formulário de cadastro de itens
│   ├── inventario.php  # Exibe o inventário de itens
│   └── login.php       # Página de login
│
├── /scripts            # Scripts PHP que processam o cadastro e a autenticação
│   ├── autenticar.php  # Verifica as credenciais de login
│   └── salvar.php      # Processa o cadastro de novos itens
│
├── /config.php         # Contém as variáveis de configuração (usuário e senha)
├── /inventario.txt     # Arquivo onde os dados do inventário são armazenados
├── logout.php          # Desconecta o usuário e redireciona para a página de login
└── index.php           # Página inicial, que redireciona para o inventário, cadastro, inventário, login
```
