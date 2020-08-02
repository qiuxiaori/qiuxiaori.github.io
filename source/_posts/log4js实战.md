---
title: log4js实战
categories: 编程技术
---

工作啦这么久，终于有机会自己从头搭日志啦，开心～
用的是 log4js 包
<!-- more -->

#### 
```
import { configure, getLogger } from 'log4js'
import { create } from '../daos/logger'
import { PRODUCTION, TEST } from '../interfaces/constant'

const NODE_ENV = process.env.NODE_ENV

configure({
  appenders: {
    out: { type: 'stdout' },
    overDebug: { type: 'stdout' },
  },
  categories: {
    default: { appenders: ['out'], level: 'all' },
    info: { appenders: ['overDebug'], level: 'info' },
  },
})
export default class Log {
  constructor(public fileName: string) {
    this.fileName = fileName
  }

  /**
   *
   * @param location
   * @param partContent
   * @param error
   */
  public async error(location: string, partContent: Record<string, unknown>, error: Error): Promise<void> {
    await wrapLog('error', this.fileName, location, { error: error.stack, ...partContent }, error.name)
  }

  /**
   *
   * @param location
   * @param content
   */
  public async info(location: string, content: Record<string, unknown>): Promise<void> {
    await wrapLog('info', this.fileName, location, content)
  }

  /**
   *
   * @param location
   * @param content
   */
  public async debug(location: string, content: Record<string, unknown>): Promise<void> {
    await wrapLog('debug', this.fileName, location, content)
  }
}

/**
 * 根据 NODE_ENV 打印或保存日志信息
 * @param level 错误级别 debug/info/error
 * @param fileName 所在文件
 * @param location 所在方法 或 操作描述
 * @param content 日志内容
 * @param errorName 错误名称
 */
export async function wrapLog(
  level: string,
  fileName: string,
  location: string,
  content: Record<string, unknown>,
  errorName = '',
): Promise<void> {
  const contentStr = JSON.stringify(content)
  // 控制台打印
  let info = `${fileName} - ${location}: ${contentStr}`
  if (errorName) {
    info += `, errorName: ${errorName}`
  }
  if (NODE_ENV === PRODUCTION) {
    // 生产环境只输出 'debug' 以上的日志
    getLogger('overDebug')[level](info)
  }
  getLogger('out')[level](info)

  // 测试环境所有日志 或 生产环境 debug 以上的日志 存储到数据库
  const isSaveProd = NODE_ENV === PRODUCTION && (level === 'info' || level === 'error')
  const isSaveTest = NODE_ENV === TEST
  if (isSaveProd || isSaveTest) {
    await create(level, fileName, location, contentStr, errorName)
  }
}

```