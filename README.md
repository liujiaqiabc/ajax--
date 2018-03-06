# ajax--

/**
 * type        : 0 同域 ， 1 跨域
 * url         : 请求的链接接口(servlet)
 */
window.centser={
        "paycent" : {
        	"type" : "1",
        	"url" : "paycent"
        },
        "goocent"  : {
        	"type" : "1",
        	"url" : "goocent"
        },
        "concent" : {
        	"type" : "1",
        	"url" : "concent"
        },
        "ordcent" : {
        	"type" : "1",
        	"url" : "ordcent"
        },
        "bssserv" : {
        	"type" : "1",
        	"url" : "bssserv"
        },
        "abilityPlatform" : {
        	"type" : "1",
        	"url" : "abilityPlatform"
        },
        "abilityPlatformCS" : {
        	"type" : "1",
        	"url" : "abilityPlatformCS"
        },
        "file.url" : {
        	"type" : "1",
        	"url" : "file.url"
        }
}
var defaultCent = "concent";
var tokenId = "033ed2b8b2213335a073ac1217aea27f";
$.extend({
	/**
	 * urlName     : 请求的链接IP(servlet)
     * url         : 请求的链接接口(servlet)
     * param       : 请求包含的参数
     * successFunc : 响应回调函数
     * async       : 是否使用异步方式传输(默认为true)
     * errorFunc   : 错误处理函数
     */
	ajaxReq : function(urlName , url, param, successFunc, errorFunc, async){
		$(".spinner , .opa").show();
		console.log(url + "请求==" + JSON.stringify(param));
		var ajaxType = window.centser[urlName].type;//0 : 同域 , 其他 : 跨域
		if(ajaxType == "0"){
			var ajaxUrl = "/" + window.centser[urlName].url;
			$.ajax({
	            url: ajaxUrl + url,
	            type: "POST",
	            timeout: 60000,
	            dataType: "json",
	            data: JSON.stringify(param),
	            async: async != false,
	            contentType: "application/json",
	            success: function(obj){
	            	console.log(url + "返回==" + JSON.stringify(obj));
	            	$(".spinner , .opa").hide();
	            	successFunc(obj);
	            },
	            error: function(XMLHttpRequest, textstatus){
	            	$(".spinner , .opa").hide();
	            	if(errorFunc){
	            		errorFunc(XMLHttpRequest, textstatus);
	            	}
	            } 
	        });
		}else{
			var ajaxUrl = "/" + window.centser[defaultCent].url;
			param.urlName =  window.centser[urlName].url;
			param.url = url;
			ajaxUrl = "";
			$.ajax({
	            url: ajaxUrl + "/v1/invoke",
	            type: "POST",
	            timeout: 60000,
	            dataType: "json",
	            data: JSON.stringify(param),
	            async: async != false,
	            contentType: "application/json",
	            success: function(obj){
	            	console.log(url + "返回==" + JSON.stringify(obj));
	            	$(".spinner , .opa").hide();
	            	successFunc(obj);
	            },
	            error: function(XMLHttpRequest, textstatus){
	            	$(".spinner , .opa").hide();
	            	if(errorFunc){
	            		errorFunc(XMLHttpRequest, textstatus);
	            	}
	            } 
	        });
		}
	}
});
