## Baixar biblioteca Laravel

### Comandos para criação da biclioteca em um repositório pelo terminal
- Baixar biblioteca do laravel:  `composer create-project --prefer-dist laravel/laravel nome_projeto`
- Demora um tempo para baixar tudo
- Entrar na pasta laravel criada `cd nome_projeto`
- Criar chave única -> acessar em **.env**: `php artisan key:generate --ansi`
- Subir servidor: `php artisan Serve`

### Primeiros passos(São Paulo)
- Alterar horário: `config` -> `app.php` -> alterar `'timezone' => "UTL"` -> para -> `"America/Sao_Paulo"`
- Casi já exista um banco de dados, entrar em: `.env` -> `DB_DATABASE: laravel` -> alterar para -> `DB_DATABASE: nome_do_banco`

### Pesquisar comandos possíveis pelo terminal
- `php artisan `
