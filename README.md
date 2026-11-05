# Sistema Escolar

Projeto de um sistema de gestão escolar construído com Laravel. O repositório está na etapa inicial de desenvolvimento, com a estrutura do framework e as ferramentas de desenvolvimento configuradas.

## Estado atual

- Página inicial padrão do Laravel.
- Modelo de usuário com factory e seeder.
- Migrations de usuários, recuperação de senha, sessões, cache e filas.
- Testes iniciais com Pest.
- Compilação de CSS e JavaScript com Vite.

Os módulos de alunos, professores, turmas, matrículas, notas e frequência ainda não foram implementados. Também não há telas de login ou painel administrativo.

## Tecnologias e requisitos

| Ferramenta | Versão usada no projeto |
| --- | --- |
| PHP | 8.5 |
| Laravel | 13 |
| Composer | 2 |
| Node.js | 24 no ambiente de desenvolvimento; o Vite instalado aceita `^20.19.0` ou `>=22.12.0` |
| Vite | 8 |
| Tailwind CSS | 4 |
| Pest | 5 |
| Banco de dados local | SQLite |

O PHP precisa das extensões exigidas pelas dependências, incluindo `pdo_sqlite` e `sqlite3` para o banco local. As versões exatas das dependências estão registradas em `composer.lock` e `package-lock.json`.

## Instalação local

### 1. Obter o projeto

```bash
git clone https://github.com/filipe-csilva/Sistema-Escolar.git
cd Sistema-Escolar
```

### 2. Instalar as dependências

```bash
composer install
npm ci
```

O comando `npm ci` usa as versões do arquivo de lock. O projeto mantém scripts de instalação de pacotes npm desativados em `.npmrc`.

### 3. Preparar o ambiente e o banco

Os comandos abaixo criam o `.env` e o banco SQLite apenas se os arquivos ainda não existirem:

```bash
php -r "file_exists('.env') || copy('.env.example', '.env');"
php -r "file_exists('database/database.sqlite') || touch('database/database.sqlite');"
php artisan key:generate --no-interaction
php artisan migrate --no-interaction
```

O `.env.example` usa `DB_CONNECTION=sqlite`. Sessões, cache e filas também utilizam o banco de dados por padrão. Para usar outro banco, ajuste as variáveis `DB_*` no `.env` antes de executar as migrations.

Execute a geração da chave na instalação inicial. Preserve a chave existente em ambientes já configurados.

### 4. Iniciar o desenvolvimento

```bash
composer run dev
```

Esse script usa o comando de desenvolvimento integrado do Laravel. Acesse o endereço informado pelo servidor no terminal.

Como alternativa, execute cada comando em um terminal separado:

```bash
php artisan serve --no-interaction
```

```bash
npm run dev
```

Se precisar processar tarefas em fila, inicie também:

```bash
php artisan queue:work --no-interaction
```

### Windows / PowerShell

Se a política de execução bloquear `npm.ps1`, use `npm.cmd` nos comandos npm, por exemplo:

```powershell
npm.cmd ci
npm.cmd run dev
npm.cmd run build
```

## Testes

Para executar a suíte:

```bash
composer test
```

Para executar apenas um arquivo:

```bash
php artisan test --compact tests/Feature/ExampleTest.php
```

A configuração em `phpunit.xml` utiliza SQLite em memória, cache e sessões em memória e execução síncrona de filas. A suíte atual contém dois testes de exemplo; a cobertura dos módulos escolares deverá acompanhar a implementação das funcionalidades.

## Build dos assets

```bash
npm run build
```

Os arquivos compilados são gerados em `public/build`. Se aparecer o erro de manifesto do Vite ausente, execute esse comando ou mantenha `npm run dev` em execução durante o desenvolvimento.

## Estrutura do projeto

| Diretório | Responsabilidade |
| --- | --- |
| `app/` | Modelos, controllers e providers da aplicação |
| `bootstrap/` | Inicialização da aplicação |
| `config/` | Configurações dos serviços e do framework |
| `database/` | Migrations, factories e seeders |
| `public/` | Ponto de entrada web e arquivos públicos |
| `resources/` | Views Blade, CSS e JavaScript |
| `routes/` | Rotas web e comandos de console |
| `storage/` | Logs, cache e arquivos gerados em execução |
| `tests/` | Testes de funcionalidade e de unidade |

## Fluxo de contribuição

Crie uma branch para cada alteração e descreva o comportamento implementado no commit. Antes de enviar mudanças, execute os testes e, quando houver alterações no frontend, o build dos assets.

As orientações de desenvolvimento estão em [AGENTS.md](AGENTS.md). As skills do projeto estão em `.agents/skills/`.

O `.gitignore` exclui `.env`, dependências instaladas, banco SQLite local e assets compilados. Mantenha credenciais fora dos arquivos versionados e use `.env.example` como modelo de configuração, sem segredos.
