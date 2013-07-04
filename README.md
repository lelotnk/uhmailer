UHMAILER
========

	<?php if (!defined('BASEPATH')) exit('No direct script access allowed');

	require_once(APPPATH . 'libraries/phpmailer/class.phpmailer.php');

	class Uhmailer
	{

		private $CI = null;
		private $mail = null;
		private $INI = array();

		public function __construct()
		{
			$this->CI =& get_instance();

	        $this->initialize();
		}

	    public function initialize()
	    {
	        $this->INI['sitename']          = 'Site Name';
	        $this->INI['site-email-from']   = 'From Name';
	        $this->INI['site-email']        = 'from@email.com';
	        $this->INI['site-email-reply']  = 'replyto@email.com';

	        $this->INI['mailserver-host']   = 'smtp.host.com';
	        $this->INI['mailserver-port']   = '465';
	        $this->INI['mailserver-login']  = 'login@smtp.com';
	        $this->INI['mailserver-pass']   = '**************';

	        $this->mail = new PHPMailer();
	        $this->mail->SetLanguage('br');
	        $this->mail->IsSMTP();
	        $this->mail->SMTPSecure = 'ssl';
	        $this->mail->CharSet = 'utf-8';
	        $this->mail->ContentType = 'text/html';
	        $this->mail->WordWrap = 100;
	        $this->mail->SMTPDebug  = 1;

	        $this->mail->Mailer   = "smtp";
	        $this->mail->SMTPAuth = true;
	        $this->mail->Host     = $this->INI['mailserver-host'];
	        $this->mail->Username = $this->INI['mailserver-login'];
	        $this->mail->Password = $this->INI['mailserver-pass'];
	        $this->mail->Port     = $this->INI['mailserver-port'];
	    }

	    public function from($fromEmail = '', $fromName = '')
	    {
	        $this->mail->From = $fromEmail;
	        $this->mail->FromName = $fromName;
	    }

	    public function addCc($ccEmail = '', $ccName = '')
	    {
	        if (is_array($ccEmail)) {

	            foreach ($ccEmail as $email) {

	                $this->mail->AddCC($email);

	            }

	        } else {

	            $this->mail->AddCC($ccEmail, $ccName);

	        }
	    }

	    public function addbcc($bccEmail = '', $bccName = '')
	    {
	        if (is_array($bccEmail)) {

	            foreach ($bccEmail as $email) {

	                $this->mail->AddBCC($email);

	            }

	        } else {

	            $this->mail->AddBCC($bccEmail, $bccName);

	        }
	    }

		public function send($email_to, $subject, $message, $replyto = null)
	    {
	        $message = $this->getTemplate($message);

	        $this->mail->Body = $message;

	        $this->setHeader($email_to, $replyto);
	        $this->setSubject($subject);
	        $this->sendEmail();
	    }

	    private function sendEmail()
	    {
	        if (!$this->mail->Send()) {

	            echo $this->mail->ErrorInfo;
	            exit;

	        }

	        unset($this->mail);

	        $this->initialize();
	    }

	    private function setHeader($to = null, $reply = null)
	    {
	        $to = $to ? $to : $this->INI['site-email-reply'];
	        $reply = $reply ? $reply : $this->INI['site-email-reply'];

	        if (!$this->mail->From)
	            $this->INI['site-email'];

	        if ($this->mail->FromName)
	            $this->INI['site-email-from'];

	        $this->mail->AddReplyTo($reply);

	        if (is_array($to)) {

	            foreach ($to as $t) {

	                $this->mail->AddAddress($t);

	            }

	        } else {

	            $this->mail->AddAddress($to);

	        }

	    }

	    private function setSubject($subject)
	    {
	        $this->mail->Subject = $subject . ' - ' . $this->INI['sitename'];
	    }

	    private function getTemplate($message)
	    {
	        return $message;

	        // $data = array('body' => $message, 'sitename' => $this->INI['sitename']);
	        // return $this->load->view('default-email', $data, true);
	    }

	}