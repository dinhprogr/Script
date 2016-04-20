# Script
Important Unity Script
// share FB unity Example
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
//using UnityEditor.
using Facebook.Unity;
public class ShareFB : MonoBehaviour {
	string fbAppUrl = "http://www.facebook.com/dialog/feed";
	public string fbAppid;
	void Awake ()
	{
		if (!FB.IsInitialized) {
			// Initialize the Facebook SDK
			FB.Init(InitCallback, OnHideUnity);
		} else {
			// Already initialized, signal an app activation App Event
			FB.ActivateApp();
		}
	}
	
	private void InitCallback ()
	{
		if (FB.IsInitialized) {
			// Signal an app activation App Event
			FB.ActivateApp();
			// Continue with Facebook SDK
			// ...
		} else {
			Debug.Log("Failed to Initialize the Facebook SDK");
		}
	}
	
	private void OnHideUnity (bool isGameShown)
	{
		if (!isGameShown) {
			// Pause the game - we will need to hide
			Time.timeScale = 0;
		} else {
			// Resume the game - we're getting focus again
			Time.timeScale = 1;
		}
	}

	 public List<string> perms =new List<string>(){"public_profile", "email", "user_friends"};
	
	private void AuthCallback (ILoginResult result) {
		FB.LogInWithReadPermissions(perms, AuthCallback);
		if (FB.IsLoggedIn) {
			// AccessToken class will have session details
			var aToken = Facebook.Unity.AccessToken.CurrentAccessToken;
			// Print current access token's User ID
			Debug.Log(aToken.UserId);
			// Print current access token's granted permissions
			foreach (string perm in aToken.Permissions) {
				Debug.Log(perm);
			}
		} else {
			Debug.Log("User cancelled login");
		}
	}
	
	/*public void ShareCallback (IShareResult result) {
		FB.ShareLink(new Uri("https://developers.facebook.com/"),callback: ShareCallback);
		if (result.Cancelled || !String.IsNullOrEmpty(result.Error)) {
			Debug.Log("ShareLink Error: "+result.Error);
		} else if (!String.IsNullOrEmpty(result.PostId)) {
			// Print post identifier of the shared content
			Debug.Log(result.PostId);
		} else {
			// Share succeeded without postID
			Debug.Log("ShareLink success!");
		}
	}*/
	public void ShareFBcheck(string linkParameter,string nameParameter,string captionParameter,string descriptionParameter,string pictureParameter,string redirectParameter) {

		Application.OpenURL(fbAppUrl + "?app_id=" + fbAppid + "&link=" + WWW.EscapeURL(linkParameter) + "&name=" + WWW.EscapeURL(nameParameter)
		                    + "&caption=" + WWW.EscapeURL(captionParameter) + "&description=" + WWW.EscapeURL(descriptionParameter) 
		                    + "&picture=" + WWW.EscapeURL(pictureParameter) + "&redirect_uri=" + WWW.EscapeURL(redirectParameter));
		FB.FeedShare(linkCaption:"Hãy thử ngay!",linkName:"Tôi đang chơi game này");
	}
	public void shareFb() {
		ShareFBcheck ("Facebook.com","SpeedControl","Tôi đang chơi game này !","Cực kỳ dễ dàng !",null,null);
	}
}
