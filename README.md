UHMAILER
========

	<?php if (!defined('BASEPATH')) exit('No direct script access allowed');

	class SendmailController extends UH_Controller
	{

	    protected $data = array();

	    public function __construct()
	    {
	        parent::__construct();
	    }

	    public function index()
	    {
	        // Load da classe uhmailer
	        $this->load->library('uhmailer');

	        // A primeira coisa a se fazer é configurar a library.
	        // Abra o arquivo /uh_app/libraries/uhmailer.php e altere as configurações de SMTP.

	        // Variável com a mensagem a ser enviada.
	        $message = '
	            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
	            <p>Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim. Donec pede justo, fringilla vel, aliquet nec, vulputate eget, arcu. In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo.</p>
	            <p>Nullam dictum felis eu pede mollis pretium. Integer tincidunt. Cras dapibus. Vivamus elementum semper nisi. Aenean vulputate eleifend tellus. Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim. Aliquam lorem ante, dapibus in, viverra quis, feugiat a, tellus.</p>
	            <p>Phasellus viverra nulla ut metus varius laoreet. Quisque rutrum. Aenean imperdiet. Etiam ultricies nisi vel augue. Curabitur ullamcorper ultricies nisi. Nam eget dui. Etiam rhoncus.</p>
	            <p>Maecenas tempus, tellus eget condimentum rhoncus, sem quam semper libero, sit amet adipiscing sem neque sed ipsum. Nam quam nunc, blandit vel, luctus pulvinar, hendrerit id, lorem. Maecenas nec odio et ante tincidunt tempus. Donec vitae sapien ut libero venenatis faucibus. Nullam quis ante. Etiam sit amet orci eget eros faucibus tincidunt. Duis leo. Sed fringilla mauris sit amet nibh. Donec sodales sagittis magna. Sed consequat, leo eget bibendum sodales, augue velit cursus nunc,</p>
	        ';

	        // Ou você pode carregar uma view como string;
	        // O terceiro parâmetro como TRUE faz a view ser retornada como string ao invés de ir para o output.
	        // $message = $this->load->view('email-default', $this->data, true);



	        /**
	         *  OPCIONAL:
	         *  Adicionar From (Remetente) diferente do padrão
	         */
	        $this->uhmailer->from('email@from.com.br', 'Nome do Remetente');
	        //


	        /**
	         *  OPCIONAL
	         *  Adicionar destinatário em Cópia
	         */
	        $this->uhmailer->addCc('destinatario@em.copia.com.br', 'Nome do Destinatário');
	        // Ou para vários destinatários
	        $this->uhmailer->addCc(array('destinatario.1@em.copia.com.br', 'destinatario.2@em.copia.com.br', 'destinatario.3@em.copia.com.br'));
	        //


	        /**
	         *  OPCIONAL
	         *  Adicionar destinatário em Cópia Oculta
	         */
	        $this->uhmailer->addbcc('destinatario@em.copia.oculta.com.br', 'Nome do Destinatário');
	        // Ou para vários destinatários
	        $this->uhmailer->addbcc(array('destinatario.1@em.copia.oculta.com.br', 'destinatario.2@em.copia.oculta.com.br', 'destinatario.3@em.copia.oculta.com.br'));
	        //


	        // Destinaário pode ser apenas 1 (um) como string
	        // Ou pode ser um array de destinatários
	        // $destinatario = array('destinatario.1@email.com.br', 'destinatario.2@email.com.br', 'destinatario.3@email.com.br');
	        $destinatario = 'destinatario@email.com.br';

	        $assunto = 'Assunto da Mensagem';

	        $replyto = 'responder-para@opcional.com.br';

	        // Chama o método de envio de email.
	        // O quarto parâmetro "responder-para" é um email para resposta. É opcional, pode simplesmente omitir e a classe vai assumir o remetente como reply-to.
	        $this->uhmailer->send($destinatario, $assunto, $message, $replyto);


	        // Depois em produção lembre-se de desabilitar o Debug na library uhmailer.php
	        // Para isso, comente as seguintes linhas no código:
	        
	        // $this->mail->SMTPDebug  = 1;
	        
	        // echo $this->mail->ErrorInfo;
	        // exit;

	    }

	}