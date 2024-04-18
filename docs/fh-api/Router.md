# 接口配置

```php
/**
 * @title  API接口和文档 - 通用配置
 * 1：接口配置参数 (user_reg)
 * @tips   string   path：           接口路径：例：user_reg 对应 /user/reg         
 * @tips   string   title：          接口名称：用于接口文档描述展示（必填）
 * @tips   string   tags：           接口分类：用于接口在文档上分类展示(可配置多个，逗号隔开) （必填）
 * @tips   string   http_method      请求方法：http请求方法：GET\POST\PUT\DELETE（大写）（必填）
 * @tips   bool     is_free          是否公共接口：true: 不需登录，false: 需要登录 （必填）
 * @tips   bool     guest_visit      可否游客访问：true：需登录并允许游客访问，false：需登录但不允许游客访问 （非必填）
 * @tips   array    data_rules       数据校验规则：配置每个请求的参数验证规则 （非必填）
 * @tips   bool     enabled          是否启用： 默认都是启用（非必填）
 * 2：数据验证器参数 (data_rules) - Api\Utils\Validator
 * @tips   string  check             验证方法：支持多验证,竖线隔开（必填）
 * @tips   bool    canEmpty          是否为空：为空时有值就验证,无值不验证（非必填, 默认不能为空）
 * @tips   string  msgPrefix         消息前缀：提示对用户的友好度，在具体错误描述前加上消息前缀（必填）
 * @tips   int     min\max\len       数据长度：在长度区间内或者指定长度（非必填）（需确定方法内部是否支持）
 * @tips   int     than\lessThen     数据大小：是否大于小于指定数值（非必填）（需确定方法内部是否支持）
 * @tips   array   in\notIn          集合验证：是否属于某个集合 （非必填）（需确定方法内部是否支持）
 * @tips   mixin   default           默认数据：参数未传或已传未传值时，填充默认数据 （非必填）（只支持数组验证）
 * 3: 请求限制参数（request_limit：根据周期和次数，算出每次请求间隔）
 * @tips   string  limitType         限制类型：IP: 访问IP, USER: 已登录用户
 * @tips   int     limitCycle        限制周期：每多少秒
 * @tips   int     limitNum          限制数量：访问多少次
 * @author benjamin
 */
 
 ```