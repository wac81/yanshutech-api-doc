```java
package service;
import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.UnsupportedEncodingException;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.ProtocolException;
import java.net.URL;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.util.LinkedHashMap;

import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.params.CookiePolicy;
import org.apache.http.client.params.HttpClientParams;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.junit.Test;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

public class clientpost {
	
	private static final String ENCODING_UTF_8 = "utf-8";
	String my_client_id = "your_client_id";
	String my_client_secret = "your_client_secret";
	String my_email = "email";
	String my_pwd = "password";
	String host = "http://acnlp.com/api/";
	String my_ip = "103.37.158.72";
	@Test
	public void test_token() {
		String token_dict = get_token();
		JSONObject jo = JSONObject.fromObject(token_dict.toString());
		String acess_token = (String) jo.get("access_token");
		HttpPostData(acess_token);

	}
	@Test
	public void test() {
		String token_dict = get_token();
		JSONObject jo = JSONObject.fromObject(token_dict.toString());
		String access_token = (String) jo.get("access_token");
		System.out.println("post_bot_get_handle");
		post_bot_get_handle(access_token);
		System.out.println("post_kwAnalyse");
		post_kwAnalyse(access_token);
		System.out.println("post_wordFlag");
		post_wordFlag(access_token);
		System.out.println("post_sentiments");
		post_sentiments(access_token);
		System.out.println("post_similar");
		post_similar(access_token);
		System.out.println("post_abstract");
		post_abstract(access_token);
		System.out.println("post_content");
		post_content(access_token);
		System.out.println("post_classification");
		post_classification(access_token);
		
	}
	private String get_token() { 
		try {
			// 建立连接 
			String token_host = host + "token";
			URL url = new URL(token_host); 
			HttpURLConnection httpConn = (HttpURLConnection) url.openConnection(); 	 
			// //设置连接属性 
			httpConn.setDoOutput(true);// 使用 URL 连接进行输出 
			httpConn.setDoInput(true);// 使用 URL 连接进行输入 
			httpConn.setUseCaches(false);// 忽略缓存 
			httpConn.setRequestMethod("POST");// 设置URL请求方法 
			httpConn.setInstanceFollowRedirects(true); 
			httpConn.setRequestProperty("Content-Type","application/json; charset=UTF-8"); 
			httpConn.connect();
			//POST请求
			DataOutputStream out = new DataOutputStream(httpConn.getOutputStream());
			JSONObject obj = new JSONObject();
			obj.element("grant_type", "password");
			obj.element("client_id", my_client_id);
			obj.element("client_secret", my_client_secret);
			obj.element("username", my_email);
			obj.element("password", my_pwd);

			out.writeBytes(obj.toString());
			out.flush();
			out.close();
			 
			//读取响应  
			BufferedReader reader = new BufferedReader(new InputStreamReader(  
					httpConn.getInputStream()));  
			String lines;  
			StringBuffer json = new StringBuffer("");  
			while ((lines = reader.readLine()) != null) {  
				json.append(new String(lines.getBytes(),"utf-8")); 
			}  
			JSONObject jo = JSONObject.fromObject(json.toString());
			System.out.println(jo.get("access_token"));
			
		    return json.toString();
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return null;  
    } 
	private void HttpPostData(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//		HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String wordFlag_url = host +"wordFlag";
	    HttpPost httppost = new HttpPost(wordFlag_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//	    JSONObject jsonParam = new JSONObject();  
//	    jsonParam.put("nl", "这是一个测试");
//	    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl=这是一个测试数据","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  
	private void post_bot_get_handle(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//		HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String bot_url = host +"yoyoBot";
	    HttpPost httppost = new HttpPost(bot_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//	    JSONObject jsonParam = new JSONObject();  
//	    jsonParam.put("nl", "这是一个测试");
//	    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+my_ip,"utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  
	
	private void chat_with_bot(String msg, String yoyoBot_str, String  access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//		HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String bot_url = host +"yoyoBot";
	    HttpPost httppost = new HttpPost(bot_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//	    JSONObject jsonParam = new JSONObject();  
//	    jsonParam.put("nl", "这是一个测试");
//	    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    String str1 = "'yoyoBot='+yoyoBot_str&'msg'=msg";
	    StringEntity reqEntity = new StringEntity("nl="+str1,"utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String res = EntityUtils.toString(response.getEntity());
	        System.out.println(res);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	} 
	
	  
	private void post_exactCut(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"searchCut";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  
	
	private void post_kwAnalyse(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"kwAnalyse";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  

	private void post_wordFlag(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"wordFlag";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  
	
	private void post_sentiments(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"sentiments";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  
	private void post_similar(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"similar";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  	
	private void post_abstract(String access_token) {  
		try {  
		    HttpClient httpclient = new DefaultHttpClient();   
//				HttpClient client = new HttpClient();
		    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
		    String chat_url = host +"abstract";
		    HttpPost httppost = new HttpPost(chat_url);   
		    //添加http头信息  
		    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
		    httppost.addHeader("Cache-Control", "no-cache");  
		    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
		    //http post的json数据格式
//			    JSONObject jsonParam = new JSONObject();  
//			    jsonParam.put("nl", "这是一个测试");
//			    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
		    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
		    httppost.setEntity(reqEntity);     
		    HttpResponse response;  
		    response = httpclient.execute(httppost);  
		    //检验状态码，如果成功接收数据  
		    JSONObject jsonObject = null;
		    int code = response.getStatusLine().getStatusCode();  
		    System.out.println(code);
		    if (code == 200) {   
		        String rev = EntityUtils.toString(response.getEntity());
		        System.out.println(rev);
		        
		    }  
		    } catch (ClientProtocolException e) {     
		    } catch (IOException e) {     
		    } catch (Exception e) {   
		    }  
		}  	
	private void post_content(String access_token) {  
		try {  
		    HttpClient httpclient = new DefaultHttpClient();   
//				HttpClient client = new HttpClient();
		    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
		    String chat_url = host +"atc/7077233320873046578_交通拥堵向小城市蔓延 专家称规划法治化不足";
		    HttpPost httppost = new HttpPost(chat_url);   
		    //添加http头信息  
		    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
		    httppost.addHeader("Cache-Control", "no-cache");  
		    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
		    //http post的json数据格式
//			    JSONObject jsonParam = new JSONObject();  
//			    jsonParam.put("nl", "这是一个测试");
//			    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
//		    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
//		    httppost.setEntity(reqEntity);     
		    HttpResponse response;  
		    response = httpclient.execute(httppost);  
		    //检验状态码，如果成功接收数据  
		    JSONObject jsonObject = null;
		    int code = response.getStatusLine().getStatusCode();  
		    System.out.println(code);
		    if (code == 200) {   
		        String rev = EntityUtils.toString(response.getEntity());
		        System.out.println(rev);
		        
		    }  
		    } catch (ClientProtocolException e) {     
		    } catch (IOException e) {     
		    } catch (Exception e) {   
		    }  
		}  	
	private void post_classification(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"classification/";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  	
//----------------------------------vision--------------------------------------------
	private void post_FaceRecognition(String access_token) {  
	try {  
	    HttpClient httpclient = new DefaultHttpClient();   
//			HttpClient client = new HttpClient();
	    HttpClientParams.setCookiePolicy(httpclient.getParams(), CookiePolicy.BROWSER_COMPATIBILITY); 
	    String chat_url = host +"classification/";
	    HttpPost httppost = new HttpPost(chat_url);   
	    //添加http头信息  
	    httppost.addHeader("Authorization","Bearer " + access_token); //认证token  
	    httppost.addHeader("Cache-Control", "no-cache");  
	    httppost.addHeader("Content-Type", "application/x-www-form-urlencoded");  
	    //http post的json数据格式
//		    JSONObject jsonParam = new JSONObject();  
//		    jsonParam.put("nl", "这是一个测试");
//		    StringEntity entity = new StringEntity(jsonParam.toString(),"utf-8");
	    StringEntity reqEntity = new StringEntity("nl="+"明天我们一起去颐和园划船吧。","utf-8");  
	    httppost.setEntity(reqEntity);     
	    HttpResponse response;  
	    response = httpclient.execute(httppost);  
	    //检验状态码，如果成功接收数据  
	    JSONObject jsonObject = null;
	    int code = response.getStatusLine().getStatusCode();  
	    System.out.println(code);
	    if (code == 200) {   
	        String rev = EntityUtils.toString(response.getEntity());
	        System.out.println(rev);
	        
	    }  
	    } catch (ClientProtocolException e) {     
	    } catch (IOException e) {     
	    } catch (Exception e) {   
	    }  
	}  	
}

```