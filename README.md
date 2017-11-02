# QQLogin
腾讯平台的qq登录
第三方登录：
    qq登录：
	方法-：
	1.腾讯开放平台中注册账号，创建移动应用(需要审核)，得到appKey，appSecret
	2.导包，权限，activity
	3.Acticity中：
		 //传入参数APPID和全局Context上下文
       		 Tencent mTencent = Tencent.createInstance(APP_ID, LoginActivity.this.getApplicationContext());
	 	
		 /**通过这句代码，SDK实现了QQ的登录，这个方法有三个参数，第一个参数是context上下文，第二个参数SCOPO 是一个String类型的字符串，表示一些权限
                 官方文档中的说明：应用需要获得哪些API的权限，由“，”分隔。例如：SCOPE = “get_user_info,add_t”；所有权限用“all”
                 第三个参数，是一个事件监听器，IUiListener接口的实例，这里用的是该接口的实现类 */
                 BaseUiListener mIUiListener = new BaseUiListener();
                //all表示获取所有权限
                mTencent.login(LoginActivity.this, "all", mIUiListener);

		/*自定义监听器实现IUiListener接口后，需要实现的3个方法
     		  onComplete完成 onError错误 onCancel取消
   		 */
   		 private class BaseUiListener implements IUiListener {

      		  @Override
       		 public void onComplete(Object response) {
         		 ToastUtil.showToast(LoginActivity.this, "授权成功");
           		 JSONObject obj = (JSONObject) response;
           		 try {
              		  final String openID = obj.getString("openid");
              		  final String threeToken = obj.getString("access_token");
            		  String expires = obj.getString("expires_in");
               		  mTencent.setOpenId(openID);
                	  mTencent.setAccessToken(threeToken, expires);
               		  QQToken qqToken = mTencent.getQQToken();
               		  mUserInfo = new UserInfo(getApplicationContext(), qqToken);
            
               		  mUserInfo.getUserInfo(new IUiListener() {
                  		  @Override
                   		 public void onComplete(Object response) {
				   Log.i("======", "登录成功" + response.toString());
                   	 	 }

                 	   	  @Override
                   		 public void onError(UiError uiError) {
                       		   Log.i("======", "登录失败" + uiError.toString());
                   		 }

                    		  @Override
                    		public void onCancel() {
                       		   Log.i("=====", "登录取消");
                    		}
               		 });
              		} catch (JSONException e) {
                	e.printStackTrace();
           	      }
       	          }

       	 	@Override
       		 public void onError(UiError uiError) {
           	        ToastUtil.showToast(LoginActivity.this, "授权失败");

       		 }

       		 @Override
        	public void onCancel() {
           	        ToastUtil.showToast(LoginActivity.this, "授权取消");
       		 }

    	    }
	  问题：
 	  1.QQ登录授权11406失败：去QQ互联，应用调试上添加自己的qq号，如果应用通过应用宝审核，这条可以忽略

	方法二：友盟

	
