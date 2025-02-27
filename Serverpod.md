Serverpod 是一個專為 Dart 和 Flutter 應用程式設計的後端框架。它提供了一個高效能、可擴展的後端解決方案，幫助開發者輕鬆管理 API、資料庫和伺服器端邏輯。

## **Serverpod 的特點**
- **深度整合 Flutter**：專為 Flutter 應用程式設計，能與 Dart 原生交互。
- **內建 ORM（物件關聯對映）**：支援 PostgreSQL，提供簡單的資料庫操作。
- **自動生成 API**：基於 Dart 的類型安全 API，讓前後端的溝通更順暢。
- **支援 WebSocket、雲端函數**：能夠處理即時通訊和分布式計算。
- **可擴展架構**：適用於小型應用，也能擴展至大型系統。

---

## **如何安裝 Serverpod**
### **步驟 1：安裝 Dart 和 Flutter**
如果尚未安裝 Dart 和 Flutter，請先安裝：
- [Dart 官方網站](https://dart.dev/get-dart)
- [Flutter 官方網站](https://flutter.dev/docs/get-started/install)

確保你的 Dart 版本至少是 **3.0.0** 以上，可以使用以下指令檢查：
```bash
dart --version
```

---

### **步驟 2：安裝 Serverpod CLI**
Serverpod 提供了 CLI（命令列工具），用來建立與管理專案。在終端機執行：
```bash
dart pub global activate serverpod_cli
```
並確保 CLI 可用：
```bash
serverpod --version
```

---

### **步驟 3：建立 Serverpod 專案**
使用以下命令來初始化一個 Serverpod 專案：
```bash
serverpod create my_project
```
這會建立三個子專案：
1. `my_project_server`：後端 API 伺服器
2. `my_project_client`：前端 Dart 客戶端
3. `my_project_flutter`（選擇性）：Flutter 應用程式（如果有使用 Flutter）

進入專案資料夾：
```bash
cd my_project
```

---

### **步驟 4：設定資料庫**
Serverpod 預設使用 **PostgreSQL** 來存儲數據。請先安裝 PostgreSQL，然後建立資料庫：
```sql
CREATE DATABASE my_project;
```

在 `my_project_server/config/development.yaml` 設定你的資料庫連線：
```yaml
database:
  host: localhost
  port: 5432
  name: my_project
  user: postgres
  password: your_password
```

---

### **步驟 5：啟動伺服器**
確保 PostgreSQL 正常運行後，執行以下指令來啟動伺服器：
```bash
cd my_project_server
dart bin/main.dart
```
如果成功，你應該會看到：
```
Server running on port 8080
```

---

## **如何使用 Serverpod**
### **1. 定義 API**
在 `my_project_server/lib/src/endpoints` 中新增一個 API，例如 `hello.dart`：
```dart
import 'package:serverpod/serverpod.dart';

class HelloEndpoint extends Endpoint {
  Future<String> sayHello(Session session, String name) async {
    return 'Hello, $name!';
  }
}
```
然後在 `endpoints.dart` 中註冊：
```dart
import 'package:serverpod/serverpod.dart';
import 'hello.dart';

class Endpoints extends EndpointDispatch {
  @override
  void initializeEndpoints(Server server) {
    var endpoints = <String, Endpoint>{
      'hello': HelloEndpoint(),
    };
    connectors = EndpointConnectors.fromEndpoints(endpoints);
  }
}
```

---

### **2. 生成 API 客戶端**
回到專案根目錄執行：
```bash
serverpod generate
```
這會根據後端 API 生成對應的 Dart 客戶端代碼。

---

### **3. 在 Flutter / Dart 客戶端呼叫 API**
在 `my_project_client` 專案中，你可以這樣呼叫 API：
```dart
import 'package:my_project_client/my_project_client.dart';

void main() async {
  var client = Client('http://localhost:8080/')
    ..connectTimeout = Duration(seconds: 10);

  var response = await client.hello.sayHello('Alice');
  print(response); // Hello, Alice!
}
```

---

## **結論**
Serverpod 是一個功能強大的 Dart 後端框架，特別適合與 Flutter 搭配使用。它簡化了後端開發，讓開發者能夠專注於業務邏輯，而不需要處理繁瑣的後端設定。如果你正在尋找一個與 Flutter 深度整合的後端解決方案，Serverpod 會是一個不錯的選擇！ 🚀