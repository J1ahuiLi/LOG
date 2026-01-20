# FinalShell 所有版本（含 4.5、4.6）离线激活码生成详解

## 为什么需要离线激活码？

FinalShell 是一款支持多平台的远程终端管理工具，但它的 官网经常失联，导致激活服务器无法连接。在某些网络受限的环境下，在线激活经常失败或超时，此时就需要使用“离线授权码”进行手动激活。

实际上，不同版本的 FinalShell 在生成授权码时使用了不同的加密规则，但它们的本质是：哈希 + 盐值。

https://cuanmu.com/237.html

## 算法核心思路

- **离线授权码本质是对「机器码 + 盐」进行散列处理后，从结果中截取某一段作为激活码：**

| 版本范围 | 使用算法  | 盐值构造                           |
| -------- | --------- | ---------------------------------- |
| < 3.9.6  | MD5       | `61305 + 机器码 + 8552`（高级）    |
|          |           | `2356 + 机器码 + 13593`（专业）    |
| ≥ 3.9.6  | Keccak384 | `机器码 + 特定字符串`              |
| 4.5～4.6 | Keccak384 | `机器码 + 特定盐（如 wcegS3gzA$）` |

## 浏览器版激活码计算工具

- 使用 HTML + JavaScript（CryptoJS）计算

  ```html
  <!DOCTYPE html>
  <html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <title>FinalShell 离线激活码生成 </title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"></script>
    <style>
      body {font-family: sans-serif; padding: 2rem; background: #f5f5f5;}
      .box {background: white; padding: 2rem; border-radius: 8px; max-width: 500px; margin: auto; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
      input, button {padding: 0.5rem; width: 100%; margin-top: 1rem;}
      .result {margin-top: 1.5rem; white-space: pre-wrap;}
    </style>
  </head>
  <body>
    <div class="box">
      <h2 id='pk-menu-3'> 离线激活码生成器 </h2>
      <input id="machineCode" type="text" placeholder=" 请输入机器码..." />
      <button onclick="generate()"> 生成授权码 </button>
      <div class="result" id="output"></div>
    </div>
   
    <script>
      function md5(str) {return CryptoJS.MD5(str).toString();}
   
      function keccak384(str) {return CryptoJS.SHA3(str, { outputLength: 384}).toString();}
   
      function generate() {const code = document.getElementById('machineCode').value.trim();
        const out = document.getElementById('output');
        let res = '';
   
        res += 'FinalShell < 3.9.6\n';
        res += '高级版:' + md5('61305' + code + '8552').slice(8, 24) + '\n';
        res += '专业版:' + md5('2356' + code + '13593').slice(8, 24) + '\n\n';
   
        res += 'FinalShell ≥ 3.9.6\n';
        res += '高级版:' + keccak384(code + 'hSf(78cvVlS5E').slice(12, 28) + '\n';
        res += '专业版:' + keccak384(code + 'FF3Go(*Xvbb5s2').slice(12, 28) + '\n\n';
   
        res += 'FinalShell 4.5\n';
        res += '高级版:' + keccak384(code + 'wcegS3gzA$').slice(12, 28) + '\n';
        res += '专业版:' + keccak384(code + 'b(xxkHn%z);x').slice(12, 28) + '\n';
   
        res += 'FinalShell 4.6\n';
        res += '高级版:' + keccak384(code + 'csSf5*xlkgYSX,y').slice(12, 28) + '\n';
        res += '专业版:' + keccak384(code + 'Scfg*ZkvJZc,s,Y').slice(12, 28) + '\n';
   
        out.textContent = res;
      }
    </script>
  </body>
  </html>
   
  
  ```

  

## Python 实现

- 使用 pycryptodome 进行哈希计算

```python
from Crypto.Hash import MD5, keccak

def calc_md5(data: str) -> str:
    return MD5.new(data.encode()).hexdigest()

def calc_keccak384(data: str) -> str:
    return keccak.new(data=data.encode(), digest_bits=384).hexdigest()

def show_activation_codes(machine_id: str):
    print('FinalShell < 3.9.6')
    print('🟡 高级版:', calc_md5(f'61305{machine_id}8552')[8:24])
    print('🟢 专业版:', calc_md5(f'2356{machine_id}13593')[8:24])

    print('FinalShell ≥ 3.9.6')
    print('🟡 高级版:', calc_keccak384(f'{machine_id}hSf(78cvVlS5E')[12:28])
    print('🟢 专业版:', calc_keccak384(f'{machine_id}FF3Go(*Xvbb5s2')[12:28])

    print('FinalShell 4.5')
    print('🟡 高级版:', calc_keccak384(f'{machine_id}wcegS3gzA$')[12:28])
    print('🟢 专业版:', calc_keccak384(f'{machine_id}b(xxkHn%z);x')[12:28])

    print('FinalShell 4.6')
    print('🟡 高级版:', calc_keccak384(f'{machine_id}csSf5*xlkgYSX,y')[12:28])
    print('🟢 专业版:', calc_keccak384(f'{machine_id}Scfg*ZkvJZc,s,Y')[12:28])

    if __name__ == '__main__':
        user_input = input('请输入机器码:').strip()
        show_activation_codes(user_input)
```