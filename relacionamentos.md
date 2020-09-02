[Criando Tabelas](https://github.com/amandamcmolina/laravel/blob/master/criandoTabelasLaravel.md)

# Relacionamentos

### Relacionamento (N:N)
##### Exemplo: tabela `projeto_categoria`

- Entrar no model Projeto em app/Projeto.php
- Adicionar relaçionamento em nova função com **nome_da_tabela** a ser associada (no caso, categorias)
- De preferência, as tabelas pivot devem conter os nomes das duas tabelas que serão associadas em ordem alfabética.
      No nosso caso, não fizemos isso: `projeto_categoria`, então, precisaremos passar o nome da tabela como parâmetro 
      do `belongsToMany('App\Categoria', 'projeto_categoria')`. E caso, o nome da tabela foreignId não seja o convecional, 
      adicionar também à estrutura `belongsToMany('App\Categoria', 'projeto_categoria', 'id referente ao model atual', 'id referente ao model de relacionamento');`.

utilizar `belongsToMany(App\Model)`

      public function categorias()
      {
        return $this->belongsToMany('App\Categoria', 'projeto_categoria', 'categoria_id', 'projeto_id');
      }
      
- Entrar no model Categoria em app/Categoria.php
- Adicionar relacionamento

      public function projetos()
      {
        return $this->belongsToMany('App\Projeto', 'projeto_categoria', 'projeto_id', 'categoria_id');
      }
      
### Relacionamento (1:N)
##### Exemplo: tabelas `users` e `projetos` (user no sentido de criador do projeto)

- Entrar no model de User em app\User.php
- Adicionar relacionamento: **1 user pode criar N projetos (user tem N projetos)**

usar `hasMany('App\Model')`
  
      public function projetos()
      {
        return this->hasMany('App\pProjeto');
      }
- Entrar no Model de Projeto em app\Projeto.php
- Adicionar relaciomento: **1 Projeto só pode ter 1 user_criador(projeto pertence 1 user criador)**

usar `belongsTo('App\Model')`

Agora é necessário fazer um teste: pegar o nome do método `user` e adionar `_id` = `user_id`. Se `user_id`não for o nome da foreignId dada na tabela Pivot, 
devemos adicionar o nome da foreingId personalizada ao `belongsTo('App\User', 'personalizado_id')`.

      public function user()
      {
        return $this->belongsTo('App\User', 'user_id');
      }

  
