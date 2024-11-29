---
title: 项目更新，如何提醒用户更新
---

# 微信小程序更新提醒(uniapp)

在uniapp中存在一个API,updateManager，用于管理小程序更新。

```js
//App.vue
import { onLaunch } from '@dcloudio/uni-app'

const updateApp = ()=>{
	const updateManager = uni.getUpdateManager();
	
    //当向小程序后台请求完新版本信息，会进行回调
	updateManager.onCheckForUpdate((res)=>{
		console.log(res.hasUpdate);
	})
	
	
	//当新版本下载完成，会进行回调
	updateManager.onUpdateReady(function (res) {
	    uni.showModal({
	      title: '更新提示',
	      content: '新版本已经准备好，是否重启应用？',
	      success(res) {
	        if (res.confirm) {
	          // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
	          updateManager.applyUpdate();
	        } else if (res.cancel) {
	          console.log('用户点击取消，不更新');
	        }
	      }
	    });
	  });
	
    //当新版本下载失败，会进行回调
	updateManager.onUpdateFailed(function (res) {
	    // 新的版本下载失败
	    uni.showModal({
	      title: '已经有新版本了哟~',
	      content: '新版本已经上线啦~，请您删除当前小程序，重新搜索打开哟~',
	    })
	  });
}
onLaunch(()=>{
	updateApp()
})
```

# web应用更新提醒


# APP更新提醒