# CJSのrequire関数の雑な説明

## function requre(specifier) [internal/modules/cjs/helpers.js:66](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/helpers.js#L66)
- Module#require を呼び出します。[88行目](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/helpers.js#L88)

## Module#require(id) [internal/modules/cjs/loader.js:991](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L991)
- Module._load を呼び出します。

## Module._load(request, parent, isMain) [internal/modules/cjs/loader.js:757](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L757)
- モジュールがキャッシュされていてロード済みの場合はそれを返します。
- 名前が node: から始まる場合は、必ずビルトインモジュールをロードします。
- そうでない場合は、ビルトインモジュールのロードを試行します。  
成功したらそれを返し、失敗したら Module オブジェクトを生成し Module#load を呼び出します。[822行目](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L822)

## Module#load(filename) [internal/modules/cjs/loader.js:963](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L963)
- 対象が ESModule (mjsファイルなど) ならエラーを送出します。
- 対象のファイルが  
.js なら Module._extensions\[".js"\] を、  
.json なら Module._extensions\[".json"\] を、  
.node なら Module._extensions\[".node"\] を、呼び出します。

## Module._extensions\[".js"](module, filename) [internal/modules/cjs/loader.js:1106](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L1106)
- fs.readFileSync を使ってソースコードを読み込み、module._compile を呼び出します。

## Module._extensions\[".json"](module, filename) [internal/modules/cjs/loader.js:1154](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L1154)
- fs.readFileSync を使ってソースコードを読み込み、JSON.parse でオブジェクトに変換します。

## Module._extensions\[".node"](module, filename) [internal/modules/cjs/loader.js:1172](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L1172)
- process.dlopen を使用してモジュールをロードします。

## Module#_compile(content, filename) [internal/modules/cjs/loader.js:1051](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L1051)
- wrapSafe を呼び出して、content を関数としてコンパイルし、それを実行します。

## function wrapSafe(filename, content, cjsModuleInstance) [internal/modules/cjs/loader.js:1011](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/cjs/loader.js#L1011)
- vm.runInThisContext や vm.compileFunction を使用して、ソースコードをコンパイルします。
