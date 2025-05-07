# MVI、MVVM、MVP 介绍与区别


在 Android 开发中，`MVI`、`MVVM`、`MVP` 是三种常见的架构模式。它们主要用于 **分离 UI 和业务逻辑**，提高代码的 **可维护性** 和
**可测试性**。

---

## **1️⃣ MVP（Model-View-Presenter）**

### **📌 介绍**

MVP 是一种较早的架构模式，主要将 `View` 和 `Model` 解耦，并通过 `Presenter` 作为中间层。

- `View`（界面层）：Activity / Fragment，负责 UI 交互。
- `Presenter`（业务逻辑层）：处理业务逻辑，并通知 `View` 更新 UI。
- `Model`（数据层）：负责数据操作，例如数据库、网络请求等。

📌 **核心思想**：`View` 只负责 UI 展示，`Presenter` 负责逻辑处理，`Model` 提供数据。

---

### **📝 示例**

#### **1️⃣ 创建 Model（数据层）**

```kotlin
interface LoginModel {
    fun login(username: String, password: String, callback: (Boolean) -> Unit)
}

class LoginModelImpl : LoginModel {
    override fun login(username: String, password: String, callback: (Boolean) -> Unit) {
        callback(username == "admin" && password == "1234") // 简单模拟
    }
}
```

#### **2️⃣ 创建 View（UI 层接口）**

```kotlin
interface LoginView {
    fun showLoading()
    fun hideLoading()
    fun showLoginSuccess()
    fun showLoginError()
}
```

#### **3️⃣ 创建 Presenter（业务逻辑层）**

```kotlin
class LoginPresenter(private val view: LoginView, private val model: LoginModel) {
    fun onLoginClicked(username: String, password: String) {
        view.showLoading()
        model.login(username, password) { isSuccess ->
            view.hideLoading()
            if (isSuccess) view.showLoginSuccess() else view.showLoginError()
        }
    }
}
```

#### **4️⃣ 在 Activity 中实现 LoginView**

```kotlin
class LoginActivity : AppCompatActivity(), LoginView {
    private lateinit var presenter: LoginPresenter

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)
        presenter = LoginPresenter(this, LoginModelImpl())

        findViewById<Button>(R.id.login_button).setOnClickListener {
            presenter.onLoginClicked("admin", "1234")
        }
    }

    override fun showLoading() { /* 显示加载动画 */ }
    override fun hideLoading() { /* 隐藏加载动画 */ }
    override fun showLoginSuccess() { Toast.makeText(this, "登录成功", Toast.LENGTH_SHORT).show() }
    override fun showLoginError() { Toast.makeText(this, "登录失败", Toast.LENGTH_SHORT).show() }
}
```

### **✅ 优势**

✔ **解耦 UI 和逻辑**，`View` 只负责 UI，`Presenter` 处理业务逻辑。  
✔ `Presenter` **可单独测试**，提高可测试性。

### **❌ 缺点**

❌ **Presenter 可能变得很大**，随着功能增加，可能导致 `Presenter` 代码臃肿。

---

##  **2️⃣ MVVM（Model-View-ViewModel）**

### **📌 介绍**

MVVM 由 Google 官方推荐，借助 `LiveData` 或 `StateFlow` 进行数据绑定，使 `ViewModel` 持有数据并驱动 UI。

- `Model`（数据层）：负责数据操作。
- `ViewModel`（逻辑层）：负责数据处理，使用 `LiveData`/`StateFlow` 让 `View` 观察数据变化。
- `View`（界面层）：Activity / Fragment，观察 `ViewModel` 并更新 UI。

📌 **核心思想**：`View` 监听 `ViewModel` 的数据变化，数据驱动 UI。

---

### **📝 示例**

#### **1️⃣ 创建 ViewModel**

```kotlin
class LoginViewModel : ViewModel() {
    private val _loginResult = MutableLiveData<Boolean>()
    val loginResult: LiveData<Boolean> get() = _loginResult

    fun login(username: String, password: String) {
        _loginResult.value = (username == "admin" && password == "1234") // 模拟登录
    }
}
```

#### **2️⃣ 在 Activity 中观察 ViewModel**

```kotlin
class LoginActivity : AppCompatActivity() {
    private lateinit var viewModel: LoginViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        viewModel = ViewModelProvider(this)[LoginViewModel::class.java]

        findViewById<Button>(R.id.login_button).setOnClickListener {
            viewModel.login("admin", "1234")
        }

        viewModel.loginResult.observe(this) { isSuccess ->
            if (isSuccess) Toast.makeText(this, "登录成功", Toast.LENGTH_SHORT).show()
            else Toast.makeText(this, "登录失败", Toast.LENGTH_SHORT).show()
        }
    }
}
```

### **✅ 优势**

✔ **ViewModel 生命周期安全**，不会因 Activity 旋转而丢失数据。  
✔ `View` 只监听数据变化，**减少 UI 层代码**。  
✔ **数据驱动 UI**，逻辑更清晰。

### **❌ 缺点**

❌ **LiveData 可能导致内存泄漏**（如果没有正确清理）。  
❌ **数据流动较隐式**，初学者可能不容易理解。

---

## **3️⃣ MVI（Model-View-Intent）**

### **📌 介绍**

MVI 采用 **单向数据流**，`View` 触发 `Intent`（用户意图），`ViewModel` 处理后更新 `State`（状态），然后 `View` 监听 `State` 变化。

- `Model`（数据层）：提供数据。
- `View`（界面层）：监听 `State` 并更新 UI。
- `Intent`（用户意图）：用户的操作，如点击按钮。
- `State`（状态）：ViewModel 维护的 UI 状态，`View` 观察它。

📌 **核心思想**：UI 由 `State` 驱动，所有事件是单向的，确保数据一致性。

---

### **📝 示例**

#### **1️⃣ 定义 UI 状态**

```kotlin
data class LoginState(val isLoading: Boolean = false, val isSuccess: Boolean? = null)
```

#### **2️⃣ 创建 ViewModel**

```kotlin
class LoginViewModel : ViewModel() {
    private val _state = MutableStateFlow(LoginState())
    val state: StateFlow<LoginState> get() = _state

    fun login(username: String, password: String) {
        _state.value = LoginState(isLoading = true)
        viewModelScope.launch {
            delay(1000) // 模拟网络请求
            _state.value = LoginState(isSuccess = (username == "admin" && password == "1234"))
        }
    }
}
```

#### **3️⃣ 在 Activity 中监听状态**

```kotlin
class LoginActivity : AppCompatActivity() {
    private lateinit var viewModel: LoginViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_login)

        viewModel = ViewModelProvider(this)[LoginViewModel::class.java]

        lifecycleScope.launch {
            viewModel.state.collect { state ->
                if (state.isLoading) showLoading()
                else if (state.isSuccess == true) showSuccess()
                else showError()
            }
        }
    }
}
```

### **✅ 优势**

✔ **状态管理清晰**，避免数据不一致。  
✔ **单向数据流**，逻辑更易维护。

### **❌ 缺点**

❌ **需要理解 StateFlow / Redux 思想**，初学者不易上手。

---

## **🔥 总结**

| 架构   | 适用场景 | 优势           | 缺点             |
|------|------|--------------|----------------|
| MVP  | 小型项目 | 解耦 UI 和逻辑    | Presenter 可能变大 |
| MVVM | 官方推荐 | 数据驱动 UI      | 可能导致内存泄漏       |
| MVI  | 状态驱动 | 单向数据流，数据一致性好 | 学习曲线较陡         |

如果是 **现代 Android** 开发，**推荐 MVVM / MVI** 🚀。