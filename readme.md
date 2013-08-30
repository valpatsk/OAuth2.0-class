Core class for OAuth 2.0 implementation.

EXAMPLE:

<pre>
class LinkedInOAuth2 extends OAuth2 {
        public function __construct($access_token=''){
            $this->access_token_url = "https://www.linkedin.com/uas/oauth2/accessToken";
            $this->authorize_url = "https://www.linkedin.com/uas/oauth2/authorization";
            parent::__construct($access_token);
            $this->access_token_name='oauth2_access_token';
        }
        public function getAuthorizeUrl($client_id,$redirect_url,$scope=''){
            $additional_args = array();
            if($scope!=''){
                if(is_array($scope)){
                    $additional_args['scope']=implode(' ',$scope);
                    $additional_args['scope'] = urlencode($additional_args['scope']);
                }else{
                    $additional_args['scope'] = urlencode($scope);
                }
            }
            $additional_args['state'] = md5(time());
            return parent::getAuthorizeUrl($client_id,$redirect_url,$additional_args);
        }

    public function getAccessToken($client_id="", $secret="", $redirect_url="", $code = ""){
        $result = parent::getAccessToken($client_id, $secret, $redirect_url, $code);
        $result = json_decode($result,true); 
        if(isset($result['error'])){
            $this->error = $result['error'].' '.$result['error_description'];
            return false;
        }else{
            $this->access_token = $result['access_token'];
            return $result['access_token'];
    }
}
</pre>