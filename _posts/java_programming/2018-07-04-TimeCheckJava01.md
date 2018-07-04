---
layout: default
title: "Java TimeCheck App Implementation, very simple 01"
---


Today implemented granting auto-userId and groupId for nickname(Constructor)<br>
This post is for testing and below is code-sample<br>
It will be update for korean ASAP!!<br>
Class is Group, Client, GroupManager, SystemManager, SystemMain...<br>
It's version for 0703 and its for android java code<br>


```java
	public Client(String nickName) {
		this.nickName = nickName;
		Random random = new Random();
		int temp = random.nextInt(100000);
		boolean duplicated = true;
		while(duplicated) {
			for (Client before : SystemManager.getSystemInstance().getAllMember()) {
				if (before.getMemId() == temp) {
					temp++;
					break;
				}
			}
			duplicated = false;
		}
		this.memId = temp;
		System.out.println(nickName+" your id is "+temp+" .. ! ");
	}
```

