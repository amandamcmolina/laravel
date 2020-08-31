[Criando Tabelas](https://github.com/amandamcmolina/laravel/blob/master/criandoTabelasLaravel.md)

# Relacionamentos

### Relacionamento (N:N)
##### Exemplo: tabela `projeto_categoria`

- Entrar no model Projeto em app/Projeto.php
- Adicionar relaçionamento em nova função com **nome_da_tabela** a ser associada (no caso, categorias)

utilizar `belongsToMany(App\Model)`

      public function categorias()
      {
        return $this->belongsToMany('App\Categoria');
      }
      
- Entrar no model Categoria em app/Categoria.php
- Adicionar relacionamento

      public function projetos()
      {
        return $this->belongsToMany('App\Projeto');
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

      public function users()
      {
        return $this->belongsTo('App\User');
      }

  
