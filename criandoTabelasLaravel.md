# criandoTabelasLaravel

### Criar Banco de dados: nomeDoBancoDeDados
- alterar nome do banco de dados no arquivo `.env`
      
      DB_DATABASE=nomeDoBancoDeDados
      
- importar para o seu banco de dados local as tabelas e os seeds já criados anteriormente

      php artisan migrate:fresh
      php artisan db:seed
      
      ou
      
      php artisan migrate:fresh --seed

### Criar tabela (Relação: 1:1  ou 1:N)
##### Exemplo: tabela PROJETOS
- Criando Model e Migration -> Primeira letra maiúscula e singular

      php artisan make:model -m Projeto     

- Entrar na pasta database -> migrations -> migration criada
- criar colunas que estão no nosso `diagrama do DER`

      $table->foreignId('user_id');
      $table->string('titulo');
      $table->text('descricao')->nullable();
      $table->text('localizacao')->nullable();
      $table->date('data_de_realizacao');
      $table->string('url_foto')->nullable();
      
- `->nullable`   ->  Para garantir que o database aceitará o campo em branco
- <https://laravel.com/docs/7.x/migrations>   -> Tem uma tabela com os tipos de dados: strings, text, date, foreignId

- Enviar tabelas e alterações para o banco de dados

      php artisan migrate:fresh

### Criar seed para tabelas (1:1 e 1:N)
- Criar seed com Primeira letra maiúscula e plural

      php artisan make:seed AddProjetos

- Encontrar o arquivo criado: database -> seeds > AddProjetos
- Adicionar caminho para o model(Cabeçalho da página)

      use App\Projeto
      
- E no caso da tabela `projetos` -> também adione ao cabeçalho  `use Carbon\Carbon`   
  Essa tabela pede uma entrada de data_realização, por isso utilizamos o caminho do carbon para enviar o formato da data corretamente
  
- Adicionar para cada projeto a seguinte estrutura (olhar nosso arquivo de Projetos no laravel). 
  Não é necessário adicionar o id ou a data de criação.
  **As colunas precisam estar na ordem de criação!**
  
        Projeto::create([
            "user_id" => 6,
            "titulo" => "Teste",
            'descricao' => "teste",
            "localizacao" => "teste",
            'data_de_realizacao' => Carbon::create('2000', '01', '01'),
            'url_foto' => "/img/projetos/projeto1.jpg"
        ]);
        
 - Entrar no arquivo do model criado:  app -> Projeto
   e adicionar `fillable = ['nomedacolunaUm', 'nomedacolunaDois']`
   
   **Os nomes das tabelas precisam ser identicos aos criados**
 
        protected $fillable = [
              'user_id',
              'titulo', 
              'descricao', 
              'localizacao', 
              'data_de_realizacao', 
              'url_foto'
          ];
          
- Entrar no:  database -> seeds -> DataBaseSeeder.php
  Aqui, diremos ao Laravel quais seeders serão criados e qual a ordem de criação destes. 
  Adicionar `$this->call(AddProjetos::class);`
  
      $this->call(AddUsers::class);
      $this->call(AddProjetos::class);
      
  **Prestar atenção na ordem**  ->  Neste exemplo já temos os seeds da tabela `users` e vamos adionar os da tabela `projetos`. 
  Quando criamos a tabela `projetos`, incluimos uma coluna que recebe `$table->foreignId('user_id');`, que é a chave 
  estrangeira referente ao id da tabela "users". 
  Então, faz sentido adicionar primeiro o AddUsers que preencherará o `id_user` e depois adicionar o `AddProjetos` que usará logo depois o `id_user`.
    
- Enviar os seeds para o banco de dados

      php artisan db:seed
 
 # **Observação** 
  Ao alterar qualquer parte da tabela ou do seed, realizar novamente os comandos:
  (Se só der o `migrate:fresh`, ele vai apagar os seeds)
  (Se só alterou algum seed e tentar enviar apenas com o comando `db:seed`, vai dar um erro falando que
  já existem dados iguais na tabela, principalmente na user)
    
        php artisan migrate:fresh
        php artisan db:seed
  ou
  
        php artisan migrate:fresh --seed
        
  **Obs. 2**
  Adicionar seed específico:
  
      php artisan db:seed --class=AddProjetos


## Criar tabela (Relação: N/N) - PIVOT 
   EX: tabelas: Projetos e categorias -> `Um projeto pode ter várias categorias e uma categoria pode estar em vários projetos`
   
- Criar tabela pivot projeto_categoria (Nomes no singular) -> Como é tabela Pivot, **não** criar MODEL para ela

**Não criamos o model nesse caso, mas se necessário, pode ser criado sim**

        php artisan make:migration CriaTabelaPivotUserHabilidade --create=projeto_categoria

- Entrar na pasta database -> migrations -> migration criada
- Apagar Coluna `$table->id();`    //Tabela Pivot(Intermediária) **não** tem id próprio
- Criar as colunas referentes aos id's das tabelas Principais (projetos e categorias)

    ~~$table->id();~~
    
        $table->foreignId('projeto_id')->constrained();  
        $table->foreignId('categoria_id')->constrained();
        
- `constrained()` -. Precisa ser colocado mesmo que vazio, o laravel já entende que o `categoria_id` já busca a tabela "categorias". 
  Mas se, por exemplo, quisermos trocar o nome da coluna de `categoria_id` para `categoria_principal_id`, 
  vamos colocar o `constrained('categorias')`, então, o laravel saberá que precisa buscar o id dentro da tabela "categorias".
  
- Enviar tabelas e alterações para o banco de dados

      php artisan migrate:fresh
 
### Criar seed para tabelas (N:N) 
- Exemplo: tabela Pivot projeto_categoria
- Criar seed para tabela pivot 

      php artisan make:seed AddProjetosCategorias
      
- Como não criamos um model para a tabela, utilizaremos o SQL para alimentar a tabela do banco de dados
- Adicionar caminho para utilização do banco de dados

      use Illuminate\Support\Facades\DB;
      
- alimentantar o seed - dentro da funcção `run()`
- Então, o banco procurará o nome da tabela `projeto_caregoria` e inserirá os dados
      
      $now = date('Y-m-d H:i:s');
        DB::table('projeto_categoria')->insert([
            [
                  "projeto_id" => 1,
                  "categoria_id" => 1,
                  "created_at" => $now,
                  "updated_at"=> $now
            ], 
            [
                  "projeto_id" => 1,
                  "categoria_id" => 2,
                  "created_at" => $now,
                  "updated_at"=> $now
            ],
            [
                  "projeto_id" => 2,
                  "categoria_id" => 3,
                  "created_at" => $now,
                  "updated_at"=> $now
            ],
      ]);
 
- Entrar no:  database -> seeds -> DataBaseSeeder.php
- Adicionar seed à lista de criação

      $this->call(AddProjetosCategorias::class);
      
- Enviar ao bando de dados
      
      php artisan migrate:fresh --seed
      
 
 
 [RELACIONAMENTOS](https://github.com/amandamcmolina/laravel/blob/master/relacionamentos.md)
