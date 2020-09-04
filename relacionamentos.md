[Criando Tabelas](https://github.com/amandamcmolina/laravel/blob/master/criandoTabelasLaravel.md)

# Relacionamentos

### Relacionamento (N:N)
##### Exemplo: tabela `projeto_categoria`

- Entrar no model Projeto em app/Projeto.php
- Adicionar relacionamento em nova função(método) com referência à tabela que será associada (no caso, categorias). Adicionamos o `belongsToMany()` e o seu primeiro parâmetro é o caminho até a model da tabela a ser associada (Categoria).
      
      public function categorias()
      {
            return this->belongsToMany('App\Categoria');
      }
      
   O laravel tem a habilidade de olhar o nome da função e saber com qual tabela ela vai se relacionar. Então, ele vai procurar por `categoria_projeto`. Assim, em ordem alfabética. Aqui encontramos um erro. De preferência, as tabelas pivot devem conter os nomes das duas tabelas que serão associadas em ordem alfabética.
   No nosso caso, não fizemos nessa ordem -  `projeto_categoria` -  então, precisaremos passar o nome da tabela como segundo parâmetro 
   do: 
   
      belongsToMany('App\Categoria', 'projeto_categoria');
 **Atenção:** procurar nome da tabela dentro do arquivo da migration. Não no nome do arquivo!
 
 Além disso, nós não somos obrigados a colocar o nome da função exatamente como está no nome da tabela, tanto é que      ele aceitaria a função como `function categorias_principais(){ ...}`, desde que específiquemos no `belongsToMany()` o nome da tabela original. 
- Adicionar também à estrutura `belongsToMany()` os id's(nomes das colunas) da tabela intermediária, na ordem exemplicada a seguir: 

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
        return $this->belongsToMany('App\Categoria', 'projeto_categoria', 'projeto_id', 'categoria_id');
      }
      
- Ao finalizar, entrar no model Categoria em app/Categoria.php e seguir o mesmo passo a passo.
- O relacionamento criado deve ser o seguinte:

      public function projetos()
      {
        return $this->belongsToMany('App\Projeto', 'projeto_categoria', 'categoria_id', 'projeto_id');
      }
      
### Relacionamento (1:N)
##### Exemplo: tabelas `users` e `projetos` (user no sentido de criador do projeto)

- Entrar no model de User em app\User.php
- Deste lado, adicionaremos o seguinte relacionamento: **1 user pode criar N projetoS (user tem N projetos)**. 
Ou seja, quando criarmos a função, o nome desta deve estar no plural. 

usar `hasMany('App\Model')` //tem Muitos(projetos)
  
      public function projetos()
      {
        return this->hasMany('App\Projeto');
      }
      
- Entrar no Model de Projeto em app\Projeto.php
- Adicionar relaciomento: **1 Projeto só pode ter 1 user_criadoR(projeto pertence 1 user criador)**. 
Ou seja, aqui chamaremos a função no **singular**. 

usar `belongsTo('App\Model')` //pertenceAo(usuário)

      public function user()
      {
        return $this->belongsTo('App\User');
      }

- Aqui levando em conta o relacionamento 1:N, podemos concluir que não existe uma tabela pivot. Se quisermos filtrar para saber quantos projetos um usuário criou, 
podemos ir até a tabela `projetos` e filtrar pelo `user_id`.

      Schema::create('projetos', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id');
            $table->string('titulo');
            $table->text('descricao')->nullable();
            $table->text('localizacao')->nullable();
            $table->date('data_de_realizacao');
            $table->string('url_foto')->nullable();
            $table->timestamps();

        });


Agora é necessário fazer um teste com a função que recebe o belongsTo() e que está no singular: pegar o nome do método `user()` e adicionar `_id` = `user_id`. Se `user_id`não for o nome da coluna foreignId dada na tabela `projetos`, devemos adicionar o nome da coluna do foreingId personalizada ao `belongsTo('App\User', 'personalizado_id')`.

      public function user()
      {
        return $this->belongsTo('App\User', 'user_id');
      }
No nosso caso, daria certo. Mas e se o nome da coluna fosse diferente? O laravel não encontria e por isso seria necessário adicionar o nome da coluna.

      $table->foreignId('user_criador_id')->constrained('users');
      
      public function user()
      {
        return $this->belongsTo('App\User', 'user_criador_id');
      }
      


  
