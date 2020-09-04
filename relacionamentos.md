[Criando Tabelas](https://github.com/amandamcmolina/laravel/blob/master/criandoTabelasLaravel.md)

# Relacionamentos

### Relacionamento (N:N)
##### Exemplo: tabela `projeto_categoria`

- Entrar no model Projeto em app/Projeto.php
- Adicionar relacionamento em nova função(método) com referencia a tabela que será associada (no caso, categorias). Adicionamos o `belongsToMany()` e o seu primeiro parâmetro é o caminho até a model da tabela a ser associada (Categoria).
      
      public function categorias()
      {
            return this->belongsToMany('App\Categoria');
      }
      
   O laravel tem a habilidade de olhar o nome da função criada e saber com qual tabela ela vai se relacionar. Então, ele vai procurar por `categoria_projeto`. Assim, em ordem alfabética. Aqui encontramos um erro. De preferência, as tabelas pivot devem conter os nomes das duas tabelas que serão associadas em ordem alfabética.
   No nosso caso, não fizemos nessa ordem -  `projeto_categoria` -  então, precisaremos passar o nome da tabela como segundo parâmetro 
   do: 
   
      belongsToMany('App\Categoria', 'projeto_categoria');
 **Atenção: ** procurar nome da tabela dentro do arquivo da migration. Não no nome do arquivo!
 
 Além disso, nós não somos obrigados a colocar o nome da função exatamente como está no nome da tabela, tanto é que      ele aceitaria a função como `function categorias_principais(){ ...}`, desde que específiquemos no `belongsToMany()` o nome da tabela original. 
- Adicionar também à estrutura `belongsToMany()` os id's(nomes das colunas) da tabela intermediária. 

      belongsToMany('App\Categoria', 'projeto_categoria', 'id referente ao model atual', 'id referente ao model de relacionamento');

No nosso exemplo (retirado da migration projeto_categoria):
      
      Schema::create('projeto_categoria', function (Blueprint $table) {
            $table->foreignId('projeto_id')->constrained();
            $table->foreignId('categoria_id')->constrained();
            $table->timestamps();
        });
A tabela intermediária `projeto_categoria` foi criada com duas foreignId. Cada uma tem um nome de coluna para os id's.

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

  
