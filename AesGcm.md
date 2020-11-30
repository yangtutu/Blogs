---
title: .Net 5中使用AES-GCM
date: 2020-11-27
---

## AES-GCM
AES是一种对称加密算法。

GCM (Galois/Counter Mode) 指的是该对称加密采用Counter模式，并带有GMAC消息认证码。

更多信息可参阅：<https://juejin.cn/post/6844904122676690951>

## .Net 5 AES-GCM

- 生成key, nonce, tag

  ```csharp
  var key = GetRandomBytes(32);
  var nonce = GetRandomBytes(System.Security.Cryptography.AesGcm.NonceByteSizes.MaxSize);
  var tag = GetRandomBytes(System.Security.Cryptography.AesGcm.TagByteSizes.MaxSize);
  ```

<!--more-->

- 加密

  ```csharp
  var plain = Encoding.UTF8.GetBytes("Hello World!");
  var cipher = new byte[plain.Length];
  var aes = new System.Security.Cryptography.AesGcm(key);
  aes.Encrypt(nonce, plain, cipher, tag);
  var cipherBase64 = Convert.ToBase64String(cipher);
  Console.WriteLine(cipherBase64);
  ```

- 解密

  ```csharp
  var plain2 = new byte[plain.Length];
  aes.Decrypt(nonce, cipher, tag, plain2);
  var text = Encoding.UTF8.GetString(plain2);
  Console.WriteLine(text);
  ```

- 随机Key辅助方法

  ```csharp
  private static byte[] GetRandomBytes(int size)
  {
      var random = new byte[size];
      var rng = new RNGCryptoServiceProvider();
      rng.GetBytes(random);
      return random;
  }
  ```

- 完整示例

  <https://github.com/ErikXu/Blogs/blob/master/ipynb/AesGcm.ipynb>