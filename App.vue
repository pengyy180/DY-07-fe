<script>
	import { findKefu } from "./api/index";
export default {
  onLaunch: function () {
    console.log("App Launch");
  },
  onShow: function () {
    console.log("App Show");
  },
  onHide: function () {
    console.log("App Hide");
  },
  methods:{
	  checkkefu(){
		  findKefu().then((res)=>{
		  	(function(a, b, c, d, e, j, s) {
		  	        a[d] = a[d] || function() {
		  	            (a[d].a = a[d].a || []).push(arguments)
		  	        };
		  	        j = b.createElement(c),
		  	            s = b.getElementsByTagName(c)[0];
		  	        j.async = true;
		  	        j.charset = 'UTF-8';
		  	        j.src = 'https://static.meiqia.com/widget/loader.js';
		  	        s.parentNode.insertBefore(j, s);
		  	    })(window, document, 'script', '_MEIQIA');
			_MEIQIA('entId', res.result);
			
		  })
		      
		
	  }
  }
  
};
</script>

<style  lang="scss">
/*每个页面公共css */

@import "style/FontStyle.css";
@import "@/uni_modules/uview-ui/index.scss";
@import "@/components/iconfont/iconfont.css";
body {
  -webkit-text-size-adjust: none;
  // font-family: Roboto,Helvetica Neue-Light,Helvetica Neue Light,Helvetica Neue,Helvetica,Arial,Lucida Grande,sans-serif;
     font-family: Arial,Microsoft YaHei,黑体,宋体,sans-serif;
	background-color: #16151b;

}
::v-deep .uicon-arrow-left{
	color:#e6b98a !important; 
}
</style>
