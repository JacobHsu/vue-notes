Vue 3 Composition API  和  Vue 2  Options API  差異

<details><summary><b>答案</b></summary>
<p>

Vue 3 Composition API 和 Vue 2 的 Options API 是兩種編寫 Vue 組件的不同方法。它們的主要差異在於組織邏輯、響應式數據處理以及重用性等方面。以下是詳細比較：

---

## **1. 組織邏輯的方式**

### **Vue 2 (Options API)**

- 使用預定義的選項（如 `data`、`methods`、`computed`、`watch`）來組織邏輯。
- 邏輯分散在不同選項中，即使它們相關。

```javascript
export default {
  data() {
    return {
      count: 0,
    };
  },
  computed: {
    doubled() {
      return this.count * 2;
    },
  },
  methods: {
    increment() {
      this.count++;
    },
  },
};
```

### **Vue 3 (Composition API)**

- 使用 `setup` 函數將所有邏輯集中管理。
- 可以將相關邏輯（狀態、函數、計算屬性等）組織在一起，更加清晰。

```javascript
import { ref, computed } from 'vue';

export default {
  setup() {
    const count = ref(0);
    const doubled = computed(() => count.value * 2);
    const increment = () => count.value++;

    return { count, doubled, increment };
  },
};
```

**差異點：**
- **Options API**：預定義選項，邏輯分散。
- **Composition API**：以功能為單位組織，邏輯集中。

---

## **2. 重用性**

### **Vue 2 (Options API)**

- 主要通過 **Mixins** 和 **Scoped Slots** 實現邏輯重用，但可能導致代碼衝突和難以跟蹤來源。

```javascript
const myMixin = {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment() {
      this.count++;
    },
  },
};
export default {
  mixins: [myMixin],
};
```

### **Vue 3 (Composition API)**

- 使用普通的 JavaScript 函數來封裝邏輯，簡單直觀且沒有命名衝突。

```javascript
import { ref } from 'vue';

function useCounter() {
  const count = ref(0);
  const increment = () => count.value++;
  return { count, increment };
}

export default {
  setup() {
    const { count, increment } = useCounter();
    return { count, increment };
  },
};
```

**差異點：**
- **Options API**：Mixins 可能導致命名衝突，難以追蹤。
- **Composition API**：使用函數封裝邏輯，易於管理和重用。

---

## **3. 響應式數據處理**

### **Vue 2 (Options API)**

- 響應式數據由 Vue 自動處理，但需要通過 `data` 定義，且數據的結構必須是預先定義好的。
- 動態添加屬性可能無法響應。

```javascript
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    addProperty() {
      this.$set(this, 'newProperty', 'value');
    },
  },
};
```

### **Vue 3 (Composition API)**

- 使用 `ref` 或 `reactive` 創建響應式數據，更靈活。
- 動態添加數據仍保持響應性。

```javascript
import { ref, reactive } from 'vue';

export default {
  setup() {
    const count = ref(0);
    const state = reactive({ name: 'Vue 3' });

    const addProperty = () => {
      state.newProperty = 'value'; // 仍然是響應的
    };

    return { count, state, addProperty };
  },
};
```

**差異點：**
- **Options API**：響應式數據結構固定，需用 `$set` 處理動態屬性。
- **Composition API**：響應式數據靈活，支持動態屬性。

---

## **4. TypeScript 支援**

### **Vue 2 (Options API)**

- 雖然支持 TypeScript，但由於 Options API 的靜態結構，類型推斷和靈活性較差，需要額外的類型定義。

```typescript
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment(this: { count: number }) {
      this.count++;
    },
  },
};
```

### **Vue 3 (Composition API)**

- 自然支持 TypeScript，使用函數組織邏輯，類型推斷更簡單直觀。

```typescript
import { ref } from 'vue';

export default {
  setup() {
    const count = ref<number>(0);

    const increment = () => {
      count.value++;
    };

    return { count, increment };
  },
};
```

**差異點：**
- **Options API**：類型推斷複雜，需要手動定義 `this` 類型。
- **Composition API**：更符合 TypeScript 語法，類型推斷自然。

---

## **5. 生命週期鉤子**

### **Vue 2 (Options API)**

- 使用預定義的生命週期選項，如 `created`、`mounted`。

```javascript
export default {
  created() {
    console.log('Component created');
  },
  mounted() {
    console.log('Component mounted');
  },
};
```

### **Vue 3 (Composition API)**

- 使用專門的生命週期函數，如 `onMounted`、`onUnmounted`，支持多次調用。

```javascript
import { onMounted } from 'vue';

export default {
  setup() {
    onMounted(() => {
      console.log('Component mounted');
    });

    return {};
  },
};
```

**差異點：**
- **Options API**：每個生命週期鉤子只能定義一次。
- **Composition API**：同一生命週期可定義多次，更靈活。

---

## **總結**

| 特性                | Vue 2 (Options API)       | Vue 3 (Composition API)    |
|---------------------|---------------------------|----------------------------|
| **組織邏輯**       | 選項分散                 | 邏輯集中                  |
| **重用性**         | Mixins，易衝突           | 函數，靈活且無衝突        |
| **響應式數據**     | 固定結構，需 `$set`      | 靈活，支持動態屬性         |
| **TypeScript 支援** | 額外定義類型             | 更自然，推斷準確          |
| **生命週期**       | 固定函數，單一執行       | 專用函數，可多次定義       |

**選擇何種方式**取決於你的需求：
- **簡單項目或初學者**：Options API 更易於上手。
- **大型應用或複雜邏輯**：Composition API 提供更好的靈活性和可維護性。

</p>
</details>

---
