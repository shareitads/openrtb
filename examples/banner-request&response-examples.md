## Banner Request example
```json
{
    "id": "123",
    "imp": [{
        "id": "1",
        "tagid": "2051",
        "bidfloor": 0.00001,
        "bidfloorcur": "USD",
        "banner":{
            "w":640,
            "h":100,
            "wmin":640,
            "hmin":100,
            "mimes": ["image/png"]
        },
        "instl": 0
    }],
 
    "app": {
        "storeurl": "",
        "ver": "8888888",
        "publisher": {
            "id": "5e33f618-42a0-47f0-b7b1-9ad9866f3b9a"
        }
    },
    "device": {
        "ip": "1.1.1.1",
        "geo": {
            "country": "ID"
        },
        "connectiontype": 7
    }
}
```

## Banner Response Example
```json
{
    "id": "123",
    "bidid": "974cb973-75fa-4c23-a8c8-9047eb3a3d5b",
    "seatbid": [
        {
            "bid": [
                {
                    "id": "974cb973-75fa-4c23-a8c8-9047eb3a3d5b",
                    "impid": "1",
                    "price": 113.7157,
                    "nurl": "https://midas-tracker-test.hellay.net/win.fcg?viewid=e6d8578664e2f10e11afa9c3e96cb376904b686aea7c97c5b18e6f3da823cde9417749dd85181cedbf3c5bd9a24e6174258174b1cca6c0e51acb7f2c12b6eef8d68800f3b86135795dd95c467ecfb8cbed45dac6d385edfeace2c4bb73fd58515ad95edfdd80591a94f5c2104e614f75f6074d462276bebd7b799524b2e88a50360d22e85e0ac35e4324018b1731e508c8a866e1823c7edd1ea97e9f0d7f280133feb00556da790936d36e034e90cf6ee784144a30a333560d5b7a37a1e08a9e140d6870bdf379a0e81fd59fd5a59362554996950499da564ef1724dd656b1cc779609b23c9e1fb38e4b3c84a1367351b30a55a99f448c42fe29810d135fef673382359eaa02fac91d4faa355707c1461fe8b4c7564cff565fa1a65be7a8af58a0d093c3c10d0c662129e76ae1ad280d99fb5ab962c31282e7b8a2739b75ab5086e76e845f37de4fbe6a400b7d443f836167891a6c919989cdbb581d93b453ebc7e386731f1bb35836f175218f72a299417a0316138cb5edac6d58b6700e3d3648579a9d10e81b78369b654ca24c2dff38324ea7e3716a6d39379ce00235ff6ccf9cb71dfeff64107c3976e3b09147af2906c8e775778726765a583f657df36e15ed4c3590594a97582599cddb81805d75aa2f29c46bb6fb48bd92ea3ec9f2fd81497c7bdb35fdcf7f16eb99903cfc21a105d44ec9b8842dcb95d1d773042daf6bf0d86e60d1f7b8cc6101f8d560f2a96b7dc498e17cb2e74bb40a9c59fa50d02c69cdb1c5670b44bdf0ad07eef8729791340a4fb688c0f09ba3b4f8ed5973a2da9be527dbda&auction_price=${AUCTION_PRICE}&sid=__SID__",
                    "adm": "<div id=\"rnd_124289\"><a href=\"javascript:void(0);\"><img src=\"https://static-dev.rqmob.com/test/sa/20210729/61d2ea8ca39af270c790111282856d9b__FILE_CM____FILE_CM480_720____WEBP__.jpg\" /></a></div><script type=\"text/javascript\">  const impTrackers = [\"https://midas-tracker-test.hellay.net/imp.fcg?viewid=e6d8578664e2f10e11afa9c3e96cb376904b686aea7c97c5b18e6f3da823cde9417749dd85181cedbf3c5bd9a24e6174258174b1cca6c0e51acb7f2c12b6eef8d68800f3b86135795dd95c467ecfb8cbed45dac6d385edfeace2c4bb73fd58515ad95edfdd80591a94f5c2104e614f75f6074d462276bebd7b799524b2e88a50360d22e85e0ac35e4324018b1731e508c8a866e1823c7edd1ea97e9f0d7f280133feb00556da790936d36e034e90cf6ee784144a30a333560d5b7a37a1e08a9e140d6870bdf379a0e81fd59fd5a59362554996950499da564ef1724dd656b1cc779609b23c9e1fb38e4b3c84a1367351b30a55a99f448c42fe29810d135fef673382359eaa02fac91d4faa355707c1461fe8b4c7564cff565fa1a65be7a8af58a0d093c3c10d0c662129e76ae1ad280d99fb5ab962c31282e7b8a2739b75ab5086e76e845f37de4fbe6a400b7d443f836167891a6c919989cdbb581d93b453ebc7e386731f1bb35836f175218f72a299417a0316138cb5edac6d58b6700e3d3648579a9d10e81b78369b654ca24c2dff38324ea7e3716a6d39379ce00235ff6ccf9cb71dfeff64107c3976e3b09147af2906c8e775778726765a583f657df36e15ed4c3590594a97582599cddb81805d75aa2f29c46bb6fb48bd92ea3ec9f2fd81497c7bdb35fdcf7f16eb99903cfc21a105d44ec9b8842dcb95d1d773042daf6bf0d86e60d1f7b8cc6101f8d560f2a96b7dc498e17cb2e74bb40a9c59fa50d02c69cdb1c5670b44bdf0ad07eef8729791340a4fb688c0e493a2a5f9b14941a6feb757ca7e3c5d233c77&auction_price=${AUCTION_PRICE}&sid=__SID__\",\"https://midas-tracker-test.hellay.net/cpi_imp.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpU2hvdyIsInJlcXVlc3RfaWQiOiI5NzRjYjk3My03NWZhLTRjMjMtYThjOC05MDQ3ZWIzYTNkNWIiLCJzdWJfcGxhdGZvcm0iOiJjcGkiLCJjaGFubmVsIjoiT01DX1NESyIsImNsaWVudF9pcCI6IjEuMS4xLjEiLCJzY3JlZW5fc2l6ZSI6IjB4MCIsIm5ldHdvcmtfdHlwZSI6NywicGFja2FnZV9uYW1lIjoi5bCP57Gz5rWL6K-VLeadqOWwkeeRnDEiLCJjb3VudHJ5IjoiaWQiLCJQYXJhbXMiOlt7ImNhbXBhaWduX2lkIjoyODEzLCJwb3NfaWQiOiIyMDUxIiwiYWRfaWQiOjEyNDI4OSwiYWRfcGFja2FnZV9uYW1lIjoiY29tLnRva29wZWRpYS50a3BkIiwiY2lkIjozOTI2MCwiY3RfY3ZyIjowLjAxMTM3MTU2NzU0LCJiaWRfcHJpY2UiOjEwMDAwMDAwLCJlY3BtIjoxMTM3MTU2NzUsImRzcF9uYW1lIjoiU2hhcmVpdCIsImF0dHJfcGxhdGZvcm0iOjIsImlzX2F1dG9fZG93bmxvYWQiOjEsImFkc2V0X2lkIjoyMzAyLCJhZF9uYW1lIjoicnRiLWJhbm5lci0yLTI3IiwiYWRzZXRfbmFtZSI6InJ0Yi1iYW5uZXIteXN5LWZpbmFsIiwicGh5X3BvcyI6IjIwNTEifV0sIm1pZGFzX3ZlcnNpb24iOiIyLjAiLCJhcHBfaWQiOiLlsI_nsbPmtYvor5Ut5p2o5bCR55GcMSJ9&sid=__SID__\"];  const clickTrackers = [\"https://midas-tracker-test.hellay.net/clk.fcg?viewid=e6d8578664e2f10e11afa9c3e96cb376904b686aea7c97c5b18e6f3da823cde9417749dd85181cedbf3c5bd9a24e6174258174b1cca6c0e51acb7f2c12b6eef8d68800f3b86135795dd95c467ecfb8cbed45dac6d385edfeace2c4bb73fd58515ad95edfdd80591a94f5c2104e614f75f6074d462276bebd7b799524b2e88a50360d22e85e0ac35e4324018b1731e508c8a866e1823c7edd1ea97e9f0d7f280133feb00556da790936d36e034e90cf6ee784144a30a333560d5b7a37a1e08a9e140d6870bdf379a0e81fd59fd5a59362554996950499da564ef1724dd656b1cc779609b23c9e1fb38e4b3c84a1367351b30a55a99f448c42fe29810d135fef673382359eaa02fac91d4faa355707c1461fe8b4c7564cff565fa1a65be7a8af58a0d093c3c10d0c662129e76ae1ad280d99fb5ab962c31282e7b8a2739b75ab5086e76e845f37de4fbe6a400b7d443f836167891a6c919989cdbb581d93b453ebc7e386731f1bb35836f175218f72a299417a0316138cb5edac6d58b6700e3d3648579a9d10e81b78369b654ca24c2dff38324ea7e3716a6d39379ce00235ff6ccf9cb71dfeff64107c3976e3b09147af2906c8e775778726765a583f657df36e15ed4c3590594a97582599cddb81805d75aa2f29c46bb6fb48bd92ea3ec9f2fd81497c7bdb35fdcf7f16eb99903cfc21a105d44ec9b8842dcb95d1d773042daf6bf0d86e60d1f7b8cc6101f8d560f2a96b7dc498e17cb2e74bb40a9c59fa50d02c69cdb1c5670b44bdf0ad07eef8729791340a4fb688c0e396b8b6fe8f456cbed7&auction_price=${AUCTION_PRICE}&sid=__SID__\",\"https://midas-tracker-test.hellay.net/cpi_clk.fcg?log=eyJldmVudF9uYW1lIjoiQURfQ3BpQ2xpY2siLCJyZXF1ZXN0X2lkIjoiOTc0Y2I5NzMtNzVmYS00YzIzLWE4YzgtOTA0N2ViM2EzZDViIiwic3ViX3BsYXRmb3JtIjoiY3BpIiwiY2hhbm5lbCI6Ik9NQ19TREsiLCJjbGllbnRfaXAiOiIxLjEuMS4xIiwic2NyZWVuX3NpemUiOiIweDAiLCJuZXR3b3JrX3R5cGUiOjcsInBhY2thZ2VfbmFtZSI6IuWwj-exs-a1i-ivlS3mnajlsJHnkZwxIiwiY291bnRyeSI6ImlkIiwiUGFyYW1zIjpbeyJjYW1wYWlnbl9pZCI6MjgxMywicG9zX2lkIjoiMjA1MSIsImFkX2lkIjoxMjQyODksImFkX3BhY2thZ2VfbmFtZSI6ImNvbS50b2tvcGVkaWEudGtwZCIsImNpZCI6MzkyNjAsImN0X2N2ciI6MC4wMTEzNzE1Njc1NCwiYmlkX3ByaWNlIjoxMDAwMDAwMCwiZWNwbSI6MTEzNzE1Njc1LCJkc3BfbmFtZSI6IlNoYXJlaXQiLCJhdHRyX3BsYXRmb3JtIjoyLCJpc19hdXRvX2Rvd25sb2FkIjoxLCJhZHNldF9pZCI6MjMwMiwiYWRfbmFtZSI6InJ0Yi1iYW5uZXItMi0yNyIsImFkc2V0X25hbWUiOiJydGItYmFubmVyLXlzeS1maW5hbCIsInBoeV9wb3MiOiIyMDUxIn1dLCJtaWRhc192ZXJzaW9uIjoiMi4wIiwiYXBwX2lkIjoi5bCP57Gz5rWL6K-VLeadqOWwkeeRnDEifQ&sid=__SID__\",\"http://ping-test.rqmob.com/click?ad=rtb-banner-2-27&ad_id=124289&ad_type={ad_type}&adpos_id=2051&adset={adset}&adset_id=2302&advid=29&amp_app_id=8057&android_id=&app_id=小米测试-杨少瑜1&app_type=3&beyla_id=&c={c}&c_id=39260&campid=1420697&channel_pkg=小米测试-杨少瑜1&channel_pkg_ver=0&cost_currency={cost_currency}&cost_model={cost_model}&cost_value={cost_value}&country_code=id&cut_type={cut_type}&device_id=&ext_info={ext_info}&gaid=&imei={imei}&imsi={imsi}&ip=1.1.1.1&is_offline=1&is_pre_install={is_pre_install}&midas_camp_id=2813&midas_traffic_type=1&os_version=&other_category={other_category}&package_type={package_type}&pkg=com.tokopedia.tkpd&placement=2051&platform=adshonor&real_attrplat=2&remote_ip=1.1.1.1&requestid=974cb973-75fa-4c23-a8c8-9047eb3a3d5b&sid={sid}&site_id={site_id}&uagent=\"];  window.onload = function() {    let __si_adbox__ = document.getElementById(\"rnd_124289\");impTrackers.forEach(item => {      var obj = document.createElement(\"img\");  obj.src= item;  obj.style.display=\"none\";  __si_adbox__.appendChild(obj);    });    __si_adbox__.addEventListener(\"click\", function(){        clickTrackers.forEach(item =>{        let obj = document.createElement(\"img\");        obj.src = item;        obj.style.display=\"none\";        __si_adbox__.appendChild(obj);      });  window.open(\"https://play.google.com/store/apps/details?id=com.tokopedia.tkpd&hl=en\");    });  }</script>",
                    "adid": "124289",
                    "bundle": "com.tokopedia.tkpd"
                }
            ]
        }
    ],
    "cur": "USD"
}
```