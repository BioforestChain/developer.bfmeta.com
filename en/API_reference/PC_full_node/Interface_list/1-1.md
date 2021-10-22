# Description of interface parameters and return parameters

1. The full name of the interface is the function name of the interface, which is also the full name of the command line call
2. The interface is abbreviated as the abbreviated name when calling the command line.
3. The callable method refers to the method in which the interface allows to be called
4. Use when the calling method is http, and add this string before /api when using websocket.
5. Request parameters and return parameters are described using typescript grammar. If the type definition is designed, please query in <type definition>.

## Get the specified account
- The full name of the interface: `getAccountInfoAndAssets`
- Interface abbreviation: `ga`
- Callable methods: `Http, Websocket, command line, Grpc`
- Calling method: `post`
- Interface `url` address: `/api/basic/getAccountInfoAndAssets`
- Request parameters:
```typescript
interface GetAccountInfoAndAssets {
     /**Account address */
     address: string;
}
```
-Return parameters:
```typescript
interface GetAccountInfoAndAssets extends RespCommonParam {
     result: MemInfoModel.AccountInfoAndAsset;
}
``` 