ie兼容问题，  

            <table width="100%" align=left cellpadding=0 cellspacing=0 bgcolor="#fff"  style="border:1px solid #ddd;border-bottom:none;">
            {foreach from=$split_order.suborder_list item=suborder}
              <tr style="text-align:center;">
                <td style="border-bottom:1px solid #ddd;padding-left:15px;background:#FAFAFA;width:33.3%" height=30>订单号：{$order.order_sn}</td>
                <td style="border-bottom:1px solid #ddd;border-left:1px solid #ddd;background:#FAFAFA;width:33.3%">支付金额：<span style="color:#FF4777;font-size:20px;"><b>{$order.order_amount}</b></span>&nbsp;元</td>
                <td style="border-bottom:1px solid #ddd;border-left:1px solid #ddd;background:#FAFAFA;width:33.3%">{$order.shipping_name}&nbsp;&nbsp;&nbsp;{$order.best_time}</td>
              </tr>
            {/foreach}
            </table>
		<div style="margin:10px auto;width:72px;"><img class="done_img" src="themes/68ecshopcom_360buy/images/flow/done_img.png" style="margin:0 auto;" /></div>
这时候出现的问题是table元素默认是浮动到左边的，但是之后的div是直接跟随在table后面，而不是重新换行，最后发现，是因为table设置了浮动的原因，div直接无视table了，对table的style进行float:none;即可。