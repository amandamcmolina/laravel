# Criando formulário de contato (envia mensagem para email)

- Criar formulário na view `paginaInicial`
- Adicionar na action o caminho para o formulário fazer a requisição post

      <form method="post" action="/contato">
            <label for="nome">Nome</label></br>
            <input type="text" id="nome" name="nome" required></br>
            <label for="email">Email</label></br>
            <input type="text" id="email" name="email"></br>
            <label for="mensagem">Mensagem</label></br>
            <textarea name="mensagem" id="mensagem" cols="30" rows="4"></textarea></br>
            <button class="btn btn-deep-orange ml-3">Enviar</button>
      </form>
      
- No controller da página inicial, criar método que acessará os dados enviados e enviará para o email
- `(Request $request)`  --- Acessará os dados enviados

      public funtion enviaContato(Request $request)
      {
            dd($request->all());//Apagar depois! // Só para verificar se os dados estão sendo enviados
      }
      
- Criar rota para envio dos dados utilizando o método criado `enviaContato` (Ela será usada apenas para o acesso dos dados)
      
      Route::post('/contato', 'PaginaInicialController@enviaContato');
      
 
- Criar Classe para o email de contato no terminal
 
      php artisan make:mail contatoEmail
      
- Acessar nova classe criada: `App\Mail\contatoEmail`.
- Adicionar ao cabeçalho duas chamadas de caminhos: 

      use Illuminate\Mail\Mailable;
      use Illuminate\Queue\SerializesModels;
      
- Dentro da classe, declarar o atributo $dados que será formulado pelo $request do Controller e pelo método enviaDados 
      
      public $dados;
      
- adicionar `$dados` também no `__construct($dados)`

      public function __construct($dados)
      {
          $this->dados = $dados;
      }
      
- e na função build() -> informar quais conteudos serão chamados para o envio do email (no caso, o from, o subject e a página que renderizará o email)

      return $this->from($this->dados->email, $this->dados->nome)
                    ->subject("Contato Toró")
                    ->view('contato.emailContato');
                    
- Essa view `contato.emailContato`(emailContato.blade.php) será criada na pasta `view\contato` com estrutura simples
      
       <h2>Contato do Toró</h2>
      <p>Nome: {{ $dados->nome }}</p>
      <p>Email: {{ $dados->email }}</p>
      <p>Mensagem: {{ $dados->mensagem }}</p>
      
- Com os dados enviador pelo método post, o controller será capaz de guardá-los na `$request` e com eles criar um novo email

      public function enviaContato(Request $request)
      {
          Mail::to('torocultural@gmail.com')->send(new ContatoEmail($request));
          return redirect('/');
      }
      
- o `return redirect('\')` envia para a rota inicial novamente, após enviar o email.
- Agora é necessári configurar o .env. Alterar sessão de `EMAIL`
(alterar o Mail_Host, o Mail=Port = 587, Mail_username = email que será utilizado para enviar os emails / a senha do email / `MAIL_ENCRYPTION = tls`)

        MAIL_MAILER=smtp
        MAIL_HOST=smtp.gmail.com
        MAIL_PORT=587
        MAIL_USERNAME=email@gmail.com
        MAIL_PASSWORD=senha
        MAIL_ENCRYPTION=tls
        MAIL_FROM_ADDRESS=null
        MAIL_FROM_NAME="${APP_NAME}"
        
- Entrar no email do gmail e alterar as configurações de segurança para permitir o acesso a APP menos seguros

- Se necessário, entrar em `config\mail.php` e alterar

        'host' => env('MAIL_HOST', 'smtp.gmail.com')
        
- E se ainda der erro, escrever comando no terminal:

         php artisan config:cache

        
                    
                    

O vídeo sobre esse assunto no `Guia Código` me ajudou muito a conseguir realizar essa operação;




