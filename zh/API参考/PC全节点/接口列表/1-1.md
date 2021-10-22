# 接口传入参数和返回参数说明

1.  接口全称为该接口的函数名，也是命令行调用时的全称名
2.  接口简写为命令行调用时的简称名。
3.  可调用方式指该接口允许通过哪些方式进行调用
4.  调用方法为http时使用，使用websocket时在 /api 前加入该字符串。
5.  请求参数与返回参数使用typescript的语法进行描述，若设计到类型定义则在<类型定义>中查询。

## 获取指定账户
- 接口全称：`getAccountInfoAndAssets`
- 接口简写：`ga`
- 可调用方式：`Http,Websocket,命令行,Grpc`
- 调用方法：`post`
- 接口`url`地址：`/api/basic/getAccountInfoAndAssets`
- 请求参数：
```typescript
interface GetAccountInfoAndAssets { 
    /**账户地址 */
    address: string;
}
```
- 返回参数：
```typescript
interface GetAccountInfoAndAssets extends RespCommonParam { 
    result: MemInfoModel.AccountInfoAndAsset;
}
```