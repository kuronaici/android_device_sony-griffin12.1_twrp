 # J9110 的TWRP设备树

已在twrp12.1分支上编译,并经过真机运行。

但是功能并不完全，因此该仓库的内容仅供参考

**已修复：**

- 触摸

**尚存在的问题：**

- 因为内核过大，因此加入解密功能后，编译出来的boot文件可能会过大( > 64 MB )，因此也移除了对中文的支持
- 文件解密
- 分区（想先看一下怎么修复解密，就暂时没完善分区表）
- 槽位切换（bootctrl模块引入有点问题）

其他未知...





解密错误日志：

```
01-04 02:30:04.869   566   566 I keystore2: keystore2_main: Successfully registered Keystore 2.0 service.
01-04 02:30:04.869   566   566 I keystore2: keystore2_main: Joining thread pool now.
01-04 02:30:05.019   567   567 I recovery: fscrypt_initialize_systemwide_keys
01-04 02:30:05.019   567   567 I recovery: Key exists, using: /data/unencrypted/key
01-04 02:30:05.020   567   567 I recovery: Retrieving key from keymaster
01-04 02:30:05.009   567   567 I recovery: type=1400 audit(0.0:343): avc: denied { read } for name="version" dev="sda77" ino=2097155 scontext=u:r:recovery:s0 tcontext=u:object_r:unencrypted_data_file:s0 tclass=file permissive=1 ppid=1 pcomm="init" pgid=1 pgcomm="init"
01-04 02:30:05.009   567   567 I recovery: type=1400 audit(0.0:344): avc: denied { open } for path="/data/unencrypted/key/version" dev="sda77" ino=2097155 scontext=u:r:recovery:s0 tcontext=u:object_r:unencrypted_data_file:s0 tclass=file permissive=1 ppid=1 pcomm="init" pgid=1 pgcomm="init"
01-04 02:30:05.021   567   567 I recovery: retrieveKey::usesKeymaster::1
01-04 02:30:05.021   567   567 I recovery: retrieveKey::usesKeymaster::2
01-04 02:30:05.021   567   567 D ProcessState: Binder ioctl to enable oneway spam detection failed: Invalid argument
01-04 02:30:05.022   564   564 E SELinux : avc:  denied  { find } for pid=567 uid=0 name=android.system.keystore2.IKeystoreService/default scontext=u:r:recovery:s0 tcontext=u:object_r:keystore_service:s0 tclass=service_manager permissive=1
01-04 02:30:05.021     0     0 I binder  : 567:567 ioctl 40046210 7fdb505bf4 returned -22
01-04 02:30:05.022   567   567 I recovery: retrieveKey::usesKeymaster::3
01-04 02:30:05.022   567   567 I recovery: retrieveKey::usesKeymaster::4
01-04 02:30:05.022   567   567 I recovery: retrieveKey::usesKeymaster::5
01-04 02:30:05.022   567   567 I recovery: BeginKeymasterOp::1
01-04 02:30:05.022   567   567 I recovery: BeginKeymasterOp::2
01-04 02:30:05.022   567   567 I recovery: BeginKeymasterOp::3
01-04 02:30:05.022   567   567 I recovery: reading blob_file: /data/unencrypted/key/keymaster_key_blob: Invalid argument
01-04 02:30:05.022   567   567 I recovery: BeginKeymasterOp::4
01-04 02:30:05.022   567   567 I recovery: BeginKeymasterOp::5
01-04 02:30:05.023   567   567 I recovery: BeginKeymasterOp::6
01-04 02:30:05.023   567   567 I recovery: BeginKeymasterOp::7
01-04 02:30:05.023   567   567 I recovery: begin::1
01-04 02:30:05.023   566   566 I SELinux : SELinux: Loaded keystore2_key_contexts from:
01-04 02:30:05.023   566   566 I SELinux :     /system/etc/selinux/plat_keystore2_key_contexts
01-04 02:30:05.024   566   566 E SELinux : avc:  denied  { manage_blob } for  scontext=u:r:recovery:s0 tcontext=u:object_r:vold_key:s0 tclass=keystore2_key permissive=1
01-04 02:30:05.024   566   566 E SELinux : avc:  denied  { use } for  scontext=u:r:recovery:s0 tcontext=u:object_r:vold_key:s0 tclass=keystore2_key permissive=1
01-04 02:30:05.027   584   584 E KeyMasterHalDevice: Begin send cmd failed
01-04 02:30:05.027   584   584 E KeyMasterHalDevice: ret: 0
01-04 02:30:05.027   584   584 E KeyMasterHalDevice: resp->status: -62
01-04 02:30:05.049   567   567 I recovery: begin::2
01-04 02:30:05.049   567   567 I recovery: begin::3
01-04 02:30:05.049   567   567 I recovery: begin::4
01-04 02:30:05.049   567   567 I recovery: begin::5
01-04 02:30:05.049   567   567 I recovery: BeginKeymasterOp::8
01-04 02:30:05.049   567   567 I recovery: Upgrading key: /data/unencrypted/key/keymaster_key_blob
01-04 02:30:05.051   567   567 I recovery: retrieveKey::usesKeymaster::6
01-04 02:30:05.051   567   567 I recovery: retrieveKey::usesKeymaster::7
01-04 02:30:05.051   566   583 E SELinux : avc:  denied  { convert_storage_key_to_ephemeral } for  scontext=u:r:recovery:s0 tcontext=u:object_r:vold_key:s0 tclass=keystore2_key permissive=1
01-04 02:30:05.051   584   584 E KeyMasterHalDevice: legacy_export_key
01-04 02:30:05.051   584   584 E KeyMasterHalDevice: ret: 0
01-04 02:30:05.052   584   584 E KeyMasterHalDevice: resp->status: -33
01-04 02:30:05.052   566   583 E keystore2: convertStorageKeyToEphemeral export_key failed, code -33
01-04 02:30:05.052   566   583 E keystore2: keystore2::error: In convert_storage_key_to_ephemeral: Failed to retrieve ephemeral key.
01-04 02:30:05.052   566   583 E keystore2: 
01-04 02:30:05.052   566   583 E keystore2: Caused by:
01-04 02:30:05.052   566   583 E keystore2:     Error::Km(ErrorCode(-33))
01-04 02:30:05.052   567   567 E recovery: keystore2 Keystore exportKey returned service specific error: -33
01-04 02:30:05.052   567   567 E recovery: Failed to get ephemeral wrapped key
01-04 02:30:05.054   567   567 I recovery: fscrypt_initialize_systemwide_keys
01-04 02:30:05.054   567   567 I recovery: Key exists, using: /data/unencrypted/key
01-04 02:30:05.054   567   567 I recovery: Retrieving key from keymaster
01-04 02:30:05.054   567   567 I recovery: retrieveKey::usesKeymaster::1
01-04 02:30:05.054   567   567 I recovery: retrieveKey::usesKeymaster::2
01-04 02:30:05.054   567   567 I recovery: retrieveKey::usesKeymaster::3
01-04 02:30:05.054   567   567 I recovery: retrieveKey::usesKeymaster::4
01-04 02:30:05.054   567   567 I recovery: retrieveKey::usesKeymaster::5
01-04 02:30:05.054   567   567 I recovery: BeginKeymasterOp::1
01-04 02:30:05.054   567   567 I recovery: BeginKeymasterOp::2
01-04 02:30:05.054   567   567 I recovery: BeginKeymasterOp::3
01-04 02:30:05.054   567   567 I recovery: reading blob_file: /data/unencrypted/key/keymaster_key_blob: Invalid argument
01-04 02:30:05.054   567   567 I recovery: BeginKeymasterOp::4
01-04 02:30:05.054   567   567 E recovery: key mkdir /tmp/keymaster_key_blob/: File exists
01-04 02:30:05.054   567   567 I recovery: BeginKeymasterOp::5
01-04 02:30:05.054   567   567 I recovery: BeginKeymasterOp::6
01-04 02:30:05.057   584   584 E KeyMasterHalDevice: Begin send cmd failed
01-04 02:30:05.057   584   584 E KeyMasterHalDevice: ret: 0
01-04 02:30:05.057   584   584 E KeyMasterHalDevice: resp->status: -62
01-04 02:30:05.074   567   567 I recovery: begin::2
01-04 02:30:05.074   567   567 I recovery: begin::3
01-04 02:30:05.074   567   567 I recovery: begin::4
01-04 02:30:05.074   567   567 I recovery: begin::5
01-04 02:30:05.074   567   567 I recovery: BeginKeymasterOp::8
01-04 02:30:05.074   567   567 I recovery: Upgrading key: /data/unencrypted/key/keymaster_key_blob
01-04 02:30:05.075   567   567 I recovery: retrieveKey::usesKeymaster::6
01-04 02:30:05.075   567   567 I recovery: retrieveKey::usesKeymaster::7
01-04 02:30:05.076   584   584 E KeyMasterHalDevice: legacy_export_key
01-04 02:30:05.076   584   584 E KeyMasterHalDevice: ret: 0
01-04 02:30:05.076   584   584 E KeyMasterHalDevice: resp->status: -33
01-04 02:30:05.076   566   566 E keystore2: convertStorageKeyToEphemeral export_key failed, code -33
01-04 02:30:05.076   566   566 E keystore2: keystore2::error: In convert_storage_key_to_ephemeral: Failed to retrieve ephemeral key.
01-04 02:30:05.076   566   566 E keystore2: 
01-04 02:30:05.076   566   566 E keystore2: Caused by:
01-04 02:30:05.076   566   566 E keystore2:     Error::Km(ErrorCode(-33))
01-04 02:30:05.076   567   567 E recovery: keystore2 Keystore exportKey returned service specific error: -33
01-04 02:30:05.076   567   567 E recovery: Failed to get ephemeral wrapped key
01-04 02:30:05.078   567   567 I recovery: fscrypt_initialize_systemwide_keys
01-04 02:30:05.078   567   567 I recovery: Key exists, using: /data/unencrypted/key
01-04 02:30:05.078   567   567 I recovery: Retrieving key from keymaster
01-04 02:30:05.078   567   567 I recovery: retrieveKey::usesKeymaster::1
01-04 02:30:05.078   567   567 I recovery: retrieveKey::usesKeymaster::2
01-04 02:30:05.079   567   567 I recovery: retrieveKey::usesKeymaster::3
01-04 02:30:05.079   567   567 I recovery: retrieveKey::usesKeymaster::4
01-04 02:30:05.079   567   567 I recovery: retrieveKey::usesKeymaster::5
01-04 02:30:05.079   567   567 I recovery: BeginKeymasterOp::1
01-04 02:30:05.079   567   567 I recovery: BeginKeymasterOp::2
01-04 02:30:05.079   567   567 I recovery: BeginKeymasterOp::3
01-04 02:30:05.079   567   567 I recovery: reading blob_file: /data/unencrypted/key/keymaster_key_blob: File exists
01-04 02:30:05.079   567   567 I recovery: BeginKeymasterOp::4
01-04 02:30:05.079   567   567 E recovery: key mkdir /tmp/keymaster_key_blob/: File exists
01-04 02:30:05.079   567   567 I recovery: BeginKeymasterOp::5
01-04 02:30:05.079   567   567 I recovery: BeginKeymasterOp::6
01-04 02:30:05.082   584   584 E KeyMasterHalDevice: Begin send cmd failed
01-04 02:30:05.082   584   584 E KeyMasterHalDevice: ret: 0
01-04 02:30:05.082   584   584 E KeyMasterHalDevice: resp->status: -62
01-04 02:30:05.098   567   567 I recovery: begin::2
01-04 02:30:05.098   567   567 I recovery: begin::3
01-04 02:30:05.098   567   567 I recovery: begin::4
01-04 02:30:05.098   567   567 I recovery: begin::5
01-04 02:30:05.098   567   567 I recovery: BeginKeymasterOp::8
01-04 02:30:05.098   567   567 I recovery: Upgrading key: /data/unencrypted/key/keymaster_key_blob
01-04 02:30:05.100   567   567 I recovery: retrieveKey::usesKeymaster::6
01-04 02:30:05.100   567   567 I recovery: retrieveKey::usesKeymaster::7
01-04 02:30:05.101   584   584 E KeyMasterHalDevice: legacy_export_key
01-04 02:30:05.101   584   584 E KeyMasterHalDevice: ret: 0
01-04 02:30:05.101   584   584 E KeyMasterHalDevice: resp->status: -33
01-04 02:30:05.101   566   566 E keystore2: convertStorageKeyToEphemeral export_key failed, code -33
01-04 02:30:05.101   566   566 E keystore2: keystore2::error: In convert_storage_key_to_ephemeral: Failed to retrieve ephemeral key.
01-04 02:30:05.101   566   566 E keystore2: 
01-04 02:30:05.101   566   566 E keystore2: Caused by:
01-04 02:30:05.101   566   566 E keystore2:     Error::Km(ErrorCode(-33))
01-04 02:30:05.101   567   567 E recovery: keystore2 Keystore exportKey returned service specific error: -33
01-04 02:30:05.101   567   567 E recovery: Failed to get ephemeral wrapped key

```

