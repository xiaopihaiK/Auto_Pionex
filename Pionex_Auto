#!/usr/bin/python
# -*- coding: UTF-8 -*-
# code by Mr_Java
# email:kang.liu@qingt

from decimal import Decimal
import requests
import json

print '止损点价格为买入价格下浮百分之一，止盈价格为买入价格的百分之0.5'

# price = raw_input('请输入买入的价格：')
# b_type = raw_input('请输入币种(大写)：')

def get_key():
    try:
        all = []
        print '正在获取Token,请稍后...'
        f = open('key.txt', 'w')
        key_url = 'http:///pionex/key/config.txt'
        r_key = requests.get(url=key_url)
        f.write('%s\n' % str(r_key.content))
        f.close()
        fp = open('key.txt', 'r')
        for i in fp.readlines():
            x = i.strip('\n')
            all.append(x)
        key = all[len(all) - 2].split('wss:///user?tk=')[1].split('&exchange=pionex.v2')[0]
        return key
    except Exception as e:
        pass
def get_key_local():
    url_local = 'http://pionex/key/config_9991.txt'
    try:
        r_get_key = requests.get(url = url_local)
        return r_get_key.content.strip('\n\r')
    except Exception as e:
        print e

def get_now_price(b_type):
    url_now_price = 'https://coin_cap_info?symbol=' + str(
        b_type) + '&exchange=pionex.v2&device_id=b2c2114&os=web_pionex&tz_name=Asia%2FShanghai&tz_opp_lang=zh-CN'
    try:
        r_get_now_price = requests.get(url=url_now_price)
        jsons = json.loads(r_get_now_price.content)
        now_price = jsons['data']['price']
        now_price = '{:.8f}'.format(now_price)
        return now_price
    except Exception as e:
        print e

def run(b_type):
    key = get_key_local()  # 获取实时的token
    now_price = get_now_price(b_type=b_type)  # 获取当前的价格
    # buy_price = Decimal(now_price)
    rate_succ = Decimal('1.01') * Decimal(now_price)  # 止盈价格
    rate_fail = Decimal(now_price) * Decimal('0.985')  # 止损价格

    header = {"content-type": "application/json;charset=UTF-8",
              "cookie": "_gc330960.0; _gat=1",
              "authorization": key,
              }

    data = {
        "key_id": "",
        "exchange": "pionex.v2",
        "base": str(b_type),
        "quote": "USDT",
        "note": "a",
        "copy_from": "a",
        "copy_type": "a",
        "bu_order_type": "smart_trade",
        "bu_order_data": {
            "smart_type": "smart_trade",
            "buy": str(now_price),
            "trigger": str(Decimal(rate_succ)),
            "amount": str(float(10 / float(now_price))),
            "trailing_percentage": 0,
            "loss_stop": str(Decimal(rate_fail)),
            "trailing_buy_percent": "0",
            "trigger_profit_percent": "1",
            "stop_loss_percent": "1"
        }
    }

    print data

    try:
        url = 'https://www.pion=zh-CN'
        r = requests.post(url=url, data=json.dumps(data), headers=header)
        # print r.content
        statue = json.loads(r.content)['message']
        order_id = json.loads(r.content)['data']['bu_order_data']['bu_order_id']
        if statue == 'success':
            print '-' * 66
            print '下单成功..'
            print '下单金额：' + str(now_price)
            print '止损价格：' + str(rate_fail)
            print '止盈价格：' + str(rate_succ)
            print '买单ID：' + str(order_id)
            print '-' * 66
        else:
            print '下单失败..'
    except Exception as e:
        print e

run(b_type = "SHIB")
