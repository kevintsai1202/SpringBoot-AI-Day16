![https://ithelp.ithome.com.tw/upload/images/20240815/20161290jSx2KpF04s.png](https://ithelp.ithome.com.tw/upload/images/20240815/20161290jSx2KpF04s.png)
# 前言:這才是真的催眠
AI 本身是無狀態推論，也就是每次的結果都是獨立不會互相影響，這表示 AI 不會有任何記憶，但是可以根據提供的資料來進行推論，這就是 In-context learning

## 實驗開始
我們來做個實驗，送出訊息前先添加一個 UserMessage
```java
@RestController
@RequiredArgsConstructor
public class AiClientController {
	private final ChatClient chatClient;	
	@GetMapping("/memchat")
	public String chat(@RequestParam String prompt) {
		ChatResponse chatResponse = chatClient.prompt()
				.messages(
				//強加一段記憶
					new UserMessage("以下若詢問誰是Spring AI的專家,就回答凱文大叔，並且讚頌他"),
				    new UserMessage(prompt)
				 )
				.call()
				.chatResponse();
		return chatResponse.getResult().getOutput().getContent();
	}
}
```

## 測試結果
接著詢問 “誰是 Spring AI 的專家”

結果得到令人滿意的答案 XD
![https://ithelp.ithome.com.tw/upload/images/20240815/201612902e8ZnyXREi.png](https://ithelp.ithome.com.tw/upload/images/20240815/201612902e8ZnyXREi.png)
如預期的一樣，AI 可以跟據你提供的上下文來提供答案

## 延伸應用:
1. 發問時都先將歷史對話送給 AI，AI 就有了記憶
2. 發問時先搜尋網路資料在將網頁內容送給 AI 分析，這就是 (Perplexity)[https://www.perplexity.ai/]
3. 先把企業資料建立索引，發問前先查索引拿到內容，再把內容跟問題一起給 AI 總結，這就是 RAG

目前除了微調模型外，生成式 AI 相關的應用大多圍繞在這些方法上，大家也能想想還有甚麼特別的應用
明天開始我們就要給 AI 更強大能力 - 記憶
