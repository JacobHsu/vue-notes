## pinia

Pinia 就是 Vuex 的替代版，可以更好的兼容 Vue2，Vue3以及TypeScript。在Vuex的基礎上去掉了 `Mutation`，只保留了 state, getter和action。Pinia擁有更簡潔的語法，扁平化的代碼編排，符合Vue3的 Composition api

相對於以前的 vuex，pinia具有以下優勢

* 更簡單的寫法，代碼更清晰簡潔，支持 composition api 和 options api 語法
* 更完善的 typescript 支持，無需創建自定義復雜的包裝類型來支持 TypeScript，所有內容都是類型化的，並且 API 的設計方式盡可能利用 TS 類型推斷
* 非常輕量，只有1kb的大小
* 不需要再注入魔法字符串等進行調用
