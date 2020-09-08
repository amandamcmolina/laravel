# CRUD

### Criar

- Cadastro de projetos ou usuários com formulário

  Criar formulário com `method="POST"` e `action = "/projeto"`. Ele criará uma rota com método para obtenção de dados dos inputs e enviou ao BD.
  
      <form method="POST" action = "\user"> 
      @csrf
      
      </form>
      
- Colocar após abertura do formulário o `@csrf` (Default)
- Criar rota para página de criação do 
      
      Route::get('user/projeto', 'PerfilProjeto@create');

- Criar rota para acessar acessar, validadar e enviar dados

      Route::post('user/projeto', 'PerfilProjeto@catch');
      
- Criar novo projeto para ser enviado ao banco de dados

      public function store(Request $request)
      {
        $produto = new Produto;
        $produto->descricao= request('descricao'); // Enviar para a coluna descricao do modelo Produto o input de name = "descricao"
        $produto->save();
        return redirect('/projetos/{id}');
      }
      
      
## EDITAR

- Criar formulário com `method="PUT"` e `action = "/projeto"`. Ele criará uma rota com método para obtenção de dados dos inputs e enviou ao BD.
  
      <form method="POST" action = "\user"> 
      @csrf
      
      </form>


