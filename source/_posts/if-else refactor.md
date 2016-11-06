---
title: 减少if-else嵌套
categories: java
description: 在日常开发中，我们经常会用if-else条件判断。可是，在业务比较复杂的情况下，if-else嵌套会比较深，在以后维护代码的时候很容易出错。
---

在日常开发中，我们经常会用if-else条件判断。可是，在业务比较复杂的情况下，if-else嵌套会比较深，在以后维护代码的时候很容易出错。

这篇文章就介绍一个减少if-else嵌套层级的方法，在if判断得到一个结果后，使用return，直接跳出该方法。


这块代码的作用是比较app是否需要更新:
优化前的代码
        
```java

		if (sue < 0){
			if (myversion.getCoerciveUpdate().equals("0")){
				showUpdateDialog(FORCIBLY);
			} else {
				//和允许的最低版本比较
				try {
					isAllowLow = compareVersion(versionCode, myversion.getAllowVersion());
				} catch (Exception e) {

				}
				//强制更新
				if (isAllowLow < 0) {
					//小于允许最低版本，强制更新
					showUpdateDialog(FORCIBLY);
				} else {
					//非强制更新
					showUpdateDialog(NO_FORCIBLY);
				}
				//非强制更新
				showUpdateDialog(NO_FORCIBLY);
			}
		} else {
			onUpdateListener.onUpdateFinish();
			if(showToast){
				ToastUtils.showLongToast("版本已经是最新的...");
			}
		}

```

优化后的代码：

```java

		//不需要更新
		if (sue >= 0 ){
			onUpdateListener.onUpdateFinish();
			if(showToast){
				ToastUtils.showLongToast("版本已经是最新的...");
			}
			return;
		}

		//强制更新
		if(myversion.getCoerciveUpdate().equals("0")){
			showUpdateDialog(FORCIBLY);
			return;
		}

		//和允许的最低版本比较
		try {
			isAllowLow = compareVersion(versionCode, myversion.getAllowVersion());
		} catch (Exception e) {

		}

		//强制更新
		if (isAllowLow < 0){
			//小于允许最低版本，强制更新
			showUpdateDialog(FORCIBLY);
			return;
		}

		//非强制更新
		showUpdateDialog(NO_FORCIBLY);
```

代码经过优化后，if-else嵌套层级变浅了。