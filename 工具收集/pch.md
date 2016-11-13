### 获取随机颜色，并且偏离白色和黑色
```
RANDOM_COLOR [UIColor colorWithRed: (50 + arc4random_uniform(150)) / 256.0 green:(50 + arc4random_uniform(150)) / 256.0 blue:(50 + arc4random_uniform(150)) / 256.0 alpha:1]
```

### 获取屏幕的大小和宽高

```
#define RY_SCRENN_SIZE [UIScreen mainScreen].bounds.size

#define RY_SCRENN_WIDTH [UIScreen mainScreen].bounds.size.width

#define RY_SCRENN_HEIGHT [UIScreen mainScreen].bounds.size.height

```

