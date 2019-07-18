[如果你觉得能帮助到你，请给一颗小星星。谢谢！(If you think it can help you, please give it a star. Thanks!)](https://github.com/dgynfi/DYFAuthIDAndGestureLock)

[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](LICENSE)&nbsp;

## DYFAuthIDAndGestureLock

手势密码解锁 和 TouchID（指纹）/ FaceID（面容）解锁，代码简洁，高效。(Gesture passcode unlocking and TouchID (fingerprint) / FaceID (facial features) unlocking, concise code and efficient.)

## 技术交流群(群号:155353383) 

欢迎加入技术交流群，一起探讨技术问题。<br />
![](https://github.com/dgynfi/DYFAuthIDAndGestureLock/raw/master/images/qq155353383.jpg)

## 效果图

<div align=left>
<img src="https://github.com/dgynfi/DYFAuthIDAndGestureLock/raw/master/images/AuthIDAndGestureLockPreview.gif" width="40%" />
</div>

## 使用说明

- 添加Face ID隐私 (Privacy)

```
<key>NSFaceIDUsageDescription</key>
<string>验证面容ID，是否允许App访问？</string>
```

- 导入头文件 (Import Header)

```
#import "DYFAuthIDAndGestureLockSettingsController.h"
#import "DYFAuthenticationView.h"
#import "DYFSecurityHelper.h"
```

- 手势密码和TouchID/FaceID设置 (Gesture Passcode and TouchID/FaceID Settings)

```
- (IBAction)settingsAction:(id)sender {
    static BOOL pushOrPresent = YES;

    DYFAuthIDAndGestureLockSettingsController *vc = [[DYFAuthIDAndGestureLockSettingsController alloc] init];
    if (pushOrPresent) {
        [self.navigationController pushViewController:vc animated:YES];
    } else {
        // When presents view controller, please add navigation controller.
        UINavigationController *nc = [[UINavigationController alloc] initWithRootViewController:vc];
        [self presentViewController:nc animated:YES completion:NULL];
    }

    pushOrPresent = !pushOrPresent;
}
```

- 验证 (Authentication)

```
- (void)executeAuthentication {
    BOOL isAuthIDOpen      = [DYFSecurityHelper authIDOpen];
    BOOL isGestureCodeOpen = [DYFSecurityHelper gestureCodeOpen];
    if (!isAuthIDOpen && !isGestureCodeOpen) {
        return;
    }

    DYFAuthenticationType type = DYFAuthenticationTypeGesture;
    if (isAuthIDOpen) {
        type = DYFAuthenticationTypeAuthID;
    }

    DYFAuthenticationView *authView = [[DYFAuthenticationView alloc] initWithFrame:[UIScreen mainScreen].bounds authenticationType:type];
    authView.avatarImage = [UIImage imageNamed:@"cat49334.jpg"];
    [authView show];

    [authView authenticateWithCompletion:^(BOOL success) {
        if (success) {
            // 进行相应的操作
            NSLog(@"验证成功");
        }
    }];

    [authView loginOtherAccountWithCompletion:^{
        // 进行其他账户登录操作
        NSLog(@"登录其他账户");
    }];
}
```
