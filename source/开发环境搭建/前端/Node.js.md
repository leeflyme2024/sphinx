Node.js 是一个开源的 JavaScript 运行时环境，它允许开发者使用 JavaScript 编写服务器端代码。Node.js 的主要用途包括以下几个方面：

1. **Web 服务器**:
   - Node.js 可以用来创建高性能的 Web 服务器，处理 HTTP 请求和响应。
   - 使用 Node.js 可以轻松地构建 RESTful API 服务器、WebSocket 服务器等。

2. **实时通信**:
   - Node.js 支持 WebSocket 协议，非常适合实现实时通信应用，如聊天应用、在线协作工具等。

3. **微服务架构**:
   - Node.js 的轻量级特性使其非常适合构建微服务架构，可以快速开发和部署小规模的服务。

4. **API 服务器**:
   - Node.js 可以用来构建 API 服务器，作为前端应用与后端数据库之间的桥梁。

5. **命令行工具**:
   - Node.js 可以用来编写命令行工具和脚本，处理文件系统操作、自动化任务等。

6. **游戏服务器**:
   - Node.js 可以用来创建游戏服务器，处理玩家之间的实时交互。

7. **文件上传/下载服务器**:
   - Node.js 可以用来构建文件上传和下载服务器，处理文件的存储和检索。

8. **数据处理和分析**:
   - Node.js 可以用来处理大规模的数据流，进行数据分析和处理。

9. **物联网 (IoT) 应用**:
   - Node.js 可以用来构建物联网应用，处理传感器数据、设备管理和远程监控。

10. **移动应用后端**:
    - Node.js 可以作为移动应用的后端服务器，提供 API 支持和数据处理。

11. **云服务**:
    - Node.js 可以用来构建云服务组件，如微服务、API 网关等。

12. **自动化测试**:
    - Node.js 可以用来编写自动化测试脚本，进行单元测试、集成测试等。

13. **开发工具**:
    - Node.js 可以用来构建开发工具，如构建工具、打包工具、代码质量检查工具等。

14. **桌面应用**:
    - 结合 Electron 框架，Node.js 可以用来开发跨平台的桌面应用。

### 特点
Node.js 的一些关键特点包括：

- **单线程和异步 I/O**:
  - Node.js 使用单线程模型，通过异步 I/O 操作避免阻塞，提高并发性能。

- **事件驱动**:
  - Node.js 使用事件驱动模型，可以高效处理大量并发连接。

- **非阻塞 I/O**:
  - Node.js 的 I/O 操作是非阻塞的，这意味着 I/O 操作可以在后台执行，而主线程可以继续处理其他任务。

- **JavaScript**:
  - Node.js 允许使用 JavaScript 进行服务器端编程，这使得前后端代码可以使用同一种语言编写，简化了开发流程。

- **庞大的生态系统**:
  - Node.js 拥有一个庞大的 npm 生态系统，提供了大量的模块和库供开发者使用。

### 核心模块

1. **`assert`**
   - **用途**: 提供断言功能，用于测试代码是否符合预期的行为。
   - **示例**:
     ```javascript
     const assert = require('assert');
     assert.strictEqual(1 + 2, 3);
     ```

2. **`buffer`**
   - **用途**: 提供操作二进制数据的能力。
   - **示例**:
     ```javascript
     const Buffer = require('buffer').Buffer;
     const buf = Buffer.from('hello world');
     console.log(buf.toString());
     ```

3. **`child_process`**
   - **用途**: 用于创建子进程，可以用来执行外部命令或脚本。
   - **示例**:
     ```javascript
     const childProcess = require('child_process');
     childProcess.exec('echo "Hello from subprocess"', (err, stdout, stderr) => {
       if (err) {
         console.error(err);
         return;
       }
       console.log(`stdout: ${stdout}`);
       console.log(`stderr: ${stderr}`);
     });
     ```

4. **`crypto`**
   - **用途**: 提供加密算法的支持，用于加密和解密数据。
   - **示例**:
     ```javascript
     const crypto = require('crypto');
     const hash = crypto.createHash('sha256');
     hash.update('hello world');
     console.log(hash.digest('hex'));
     ```

5. **`events`**
   - **用途**: 提供事件驱动模型，用于创建和处理事件。
   - **示例**:
     ```javascript
     const EventEmitter = require('events');
     const ee = new EventEmitter();

     ee.on('messageLogged', (arg) => {
       console.log('Listener called', arg);
     });

     ee.emit('messageLogged', { id: 1, url: 'http://foo.com' });
     ```

6. **`fs`**
   - **用途**: 提供文件系统操作的功能，如读取、写入、更新、重命名、删除文件等。
   - **示例**:
     ```javascript
     const fs = require('fs');
     fs.readFile('./example.txt', 'utf8', (err, data) => {
       if (err) throw err;
       console.log(data);
     });
     ```

7. **`http` 和 `https`**
   - **用途**: 提供创建 HTTP 和 HTTPS 服务器的能力。
   - **示例**:
     ```javascript
     const http = require('http');
     const server = http.createServer((req, res) => {
       res.statusCode = 200;
       res.setHeader('Content-Type', 'text/plain');
       res.end('Hello World\n');
     });
     server.listen(3000, () => {
       console.log('Server running at http://localhost:3000/');
     });
     ```

8. **`net`**
   - **用途**: 提供网络连接的能力，用于创建 TCP 服务器和客户端。
   - **示例**:
     ```javascript
     const net = require('net');
     const server = net.createServer((socket) => {
       socket.write('Echo server\r\n');
       socket.pipe(socket);
     });
     server.listen(5000, () => {
       console.log('Server running on port 5000');
     });
     ```

9. **`os`**
   - **用途**: 提供有关操作系统的信息。
   - **示例**:
     ```javascript
     const os = require('os');
     console.log(os.platform());
     console.log(os.homedir());
     ```

10. **`path`**
    - **用途**: 提供处理文件路径的功能，如拼接路径、获取文件名等。
    - **示例**:
      ```javascript
      const path = require('path');
      const filePath = path.join(__dirname, 'example.txt');
      console.log(filePath);
      ```

11. **`process`**
    - **用途**: 提供关于当前 Node.js 进程的信息和控制。
    - **示例**:
      ```javascript
      console.log(process.version);
      process.exit(0);
      ```

12. **`querystring`**
    - **用途**: 提供编码和解码 URL 查询字符串的功能。
    - **示例**:
      ```javascript
      const querystring = require('querystring');
      const query = querystring.stringify({ foo: 'bar', baz: 'qux' });
      console.log(query); // 输出: "foo=bar&baz=qux"
      ```

13. **`stream`**
    - **用途**: 提供流式 I/O 的功能，可以用来处理大量数据。
    - **示例**:
      ```javascript
      const fs = require('fs');
      const Readable = require('stream').Readable;
      const rs = new Readable();
      rs.push('foo\n');
      rs.push('bar\n');
      rs.push(null);
      rs.pipe(fs.createWriteStream('output.txt'));
      ```

14. **`url`**
    - **用途**: 提供处理 URL 的功能。
    - **示例**:
      ```javascript
      const url = require('url');
      const parsedUrl = url.parse('http://user:pass@host.com:8080/path?query=string#hash');
      console.log(parsedUrl);
      ```

15. **`util`**
    - **用途**: 提供一些实用工具函数，如打印对象、深拷贝等。
    - **示例**:
      ```javascript
      const util = require('util');
      console.log(util.inspect({ foo: 'bar', baz: 'qux' }, { showHidden: false, depth: null }));
      ```

16. **`vm`**
    - **用途**: 提供创建独立的 JavaScript 运行环境的能力。
    - **示例**:
      ```javascript
      const vm = require('vm');
      const script = new vm.Script('3 + 4');
      console.log(script.runInNewContext());
      ```

17. **`zlib`**
    - **用途**: 提供压缩和解压缩数据的功能。
    - **示例**:
      ```javascript
      const zlib = require('zlib');
      const gzip = zlib.createGzip();
      const inp = fs.createReadStream('input.txt');
      const out = fs.createWriteStream('input.txt.gz');
      inp.pipe(gzip).pipe(out);
      ```
