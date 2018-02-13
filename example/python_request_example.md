```python
# -*- coding:utf-8 -*-
from __future__ import print_function
import requests
import json
import time
import base64
import cPickle
import urllib2
import urllib


my_client_id = "69413b5e89e09d62db4625d0453f522f"
my_client_secret = "b3036dcd185ee47a730637c3c6ab007c"
my_email = "402955987@qq.com"
my_pwd = "woleigequ"
host = "http://acnlp.com/api/"
my_ip = "103.37.158.72"


def timing(func):
    def wrapper(*args, **kwargs):
        tic = time.time()
        print("> %s, {%s} start" % (time.strftime("%X", time.localtime()), func.__name__))
        back = func(*args, **kwargs)
        print("> %s, {%s} end" % (time.strftime("%X", time.localtime()), func.__name__))
        print("> %.3fs taken for {%s}" % (time.time() - tic, func.__name__))
        return back
    return wrapper


#############################################
# token
@timing
def get_token():
    print("==============")
    print("get_token...")
    token_host = host + "token"
    payload = {
        "grant_type": "password",
        "client_id": my_client_id,
        "client_secret": my_client_secret,
        "username": my_email,
        "password": my_pwd
    }

    res = requests.post(
        url=token_host,
        data=payload
    )

    response = res.content
    print(response)
    return json.loads(response)


@timing
def exchange_token(dtoken):
    print("==============")
    print("exchange_token...")
    token_host = host + "token"
    refresh_token = dtoken["refresh_token"]
    payload = {
        "grant_type": "password",
        "client_id": my_client_id,
        "client_secret": my_client_secret,
        "username": my_email,
        "password": my_pwd,
        "refresh_token": refresh_token
    }

    res = requests.post(
        url=token_host,
        data=payload
    )

    response = res.content
    print(response)
    return json.loads(response)


@timing
def my_post(my_access_token):
    print("==============")
    print("my_post...")
    wordFlag_url = host + "wordFlag"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "这是一个测试"
    }
    res = requests.post(
        url=wordFlag_url,
        data=payload,
        headers=headers
    )

    response = res.content
    print(response)


#############################################
# chatbot
@timing
def post_bot_get_handle(my_access_token):
    """
    启动 chat bot
    :param my_access_token:
    :return:
    """
    print("==============")
    print("post_bot...")
    bot_url = host + "yoyoBot"

    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }

    # 获得句柄
    payload = {
        "nl": my_ip
    }
    res = requests.post(
        url=bot_url,
        data=payload,
        headers=headers
    )
    if res.status_code == 200:
        yoyoBot_str = json.loads(res.content)["yoyoBot"]
        print("> yoyoBot online, you can talk to her...")
    else:
        raise ValueError(res.content)
    # print(yoyoBot_str)
    yoyoBot_str = yoyoBot_str.encode("utf-8")
    # yoyoBot_obj = cPickle.loads(yoyoBot_str)
    return yoyoBot_str

@timing
def chat_with_bot(msg="推荐电影", yoyoBot_str=None, my_access_token=None):
    """
    聊天
    :param msg:
    :param yoyoBot_str:
    :param my_access_token:
    :return:
    """
    print("==============")
    chat_url = host + "yoyoBotChat"

    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded",
    }
    nl_body = {
        "yoyoBot": yoyoBot_str,
        "msg": msg
    }

    # 注意：application/x-www-form-urlencoded 就是
    # k1=v1&k2=v2...这样的key=>str_val结构, 所以不适合多级字典form数据
    # 所以要把内层字典结构编程json字符串
    payload = {
        "nl": json.dumps(nl_body)
    }

    print("[PayLoad]", payload)
    print("[URL]", chat_url)
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)


#############################################
# text
@timing
def post_exactCut(my_access_token):
    """
    分词
    :param my_access_token:
    :return:
    """
    print("==============")
    print("post_exactCut...")
    chat_url = host + "exactCut"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_searchCut(my_access_token):
    print("==============")
    print("post_searchCut...")
    chat_url = host + "searchCut"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_kwAnalyse(my_access_token):
    print("==============")
    print("post_kwAnalyse...")
    chat_url = host + "kwAnalyse"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_wordFlag(my_access_token):
    print("==============")
    print("post_wordFlag...")
    chat_url = host + "wordFlag"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_sentiments(my_access_token):
    print("==============")
    print("post_sentiments...")
    chat_url = host + "sentiments"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        # "nl": "明天我们一起去颐和园划船吧。"
        # "nl": "我们要打败恐怖主义"
        # "nl": "我喜欢喝安佳牛奶"
        # "nl": "我讨厌安佳牛奶" * 200
        # "nl": "你是个大傻逼" * 200
        "nl": "本期话题主持人：左蓉达 本报记者徐伯元回顾2014年的经济故事，畅想2015年的经济生活。在跨年之际，《大东北》发起了“总结·展望”话题，网友们纷纷留言讨论，有的谈对东北“大经济”的期盼，有的讲对身边“小经济”的愿望。无论如何，大家都希望东北经济能讲好新一年的故事。希望大东北的“大经济”更活跃@柳河本人：东北一些知名企业采取了“走出去”的发展策略，同时也采取了区域经济联动的方式，不但能分享到他处的蛋糕，也在资源共享的同时增加了竞争的资本。对东北经济的展望是，希望发挥东北老工业基地的优势，通过不同区域的资源往来，加大技术合作，使东北经济相互借力飞翔。左蓉达叫左左更亲切：希望东北经济的区域联系和包容性更强，区域内的资本流动更活跃，既能带动资源整合和经济发展，也为人力、物力、财力的自由流通创造便利条件。也希望东北经济日益加强合作，可为更多人带来创业机遇，也能为更多高校毕业生创造就业岗位，让城市更加充满竞争和活力，让年轻人能更快安家落户。期望东北人的“小经济”更踏实@木子开花02033：2014年对我影响最大的是不断出台的房改新政，在经历了新政带来的心理上的忽起忽落之后，我的收获是平和。面对市场供求关系、大经济环境及金融政策的变化，客观、平和看待才是最好的态度。@刁蛮天慈：每个人都希望自己能拥有一份理想的、收入好的工作，可是想法和现实经常有差距。我愿意兢兢业业完成每一天的工作，但美中不足的是单位不给“五险一金”待遇，又缺少选择单位的空间。作为打工者，深切期盼有更多法律维权的渠道，维护劳动者的权益。@拎包走世界：2014马路上的车越来越多，路越来越堵，真愁人！希望2015年有关部门能加大力度，多建一些立体停车场，让停车更方便，让车主们少些罚单，让路上多些畅通！东北，你能带个头不？原文连接：http://szb.dlxww.com/dlrb/html/2015-01/08/content_1107532.htm?div=-1免责声明：慧科和该网页的作者无关，不对其内容负责。该快照或内容仅为索引不代表被搜索网站的即时页面，需要查看完整资讯请点击链结。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_similar(my_access_token):
    print("==============")
    print("post_similar...")
    chat_url = host + "similar"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_abstract(my_access_token):
    print("==============")
    print("post_abstract...")
    chat_url = host + "abstract"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_content(my_access_token):
    print("==============")
    print("post_content...")
    chat_url = host + "atc/7077233320873046578_交通拥堵向小城市蔓延 专家称规划法治化不足"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }

    res = requests.get(
        url=chat_url,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_classification(my_access_token):
    print("==============")
    print("post_classification...")
    chat_url = host + "classification/"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "nl": "明天我们一起去颐和园划船吧。"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)


#############################################
# vision
def img_base64():
    file_name = '20130528025559738.jpg'
    img_type = file_name.split(".")[-1]
    f = open(file_name, 'rb')
    ls_f = "data:image/" + img_type + ":base64," + base64.b64encode(f.read())
    f.close()
    return ls_f


@timing
def post_FaceRecognition(my_access_token):
    print("==============")
    print("post_FaceRecognition...")
    chat_url = host + "FaceRecognition/"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }

    base64_str = img_base64()

    payload = "img=" + base64_str
    res = requests.get(
        url=chat_url + payload,
        # data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_ImageClassification(my_access_token):
    print("==============")
    print("post_ImageClassification...")
    chat_url = host + "ImageClassification/"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "img": "data:image/jpg;base64,/9j/4AAQSkZJRgABAQEASABIAAD/7QAsUGhvdG9zaG9wIDMuMAA4QklNBCUAAAAAABAAAAAAAAAAAAAAAAAAAAAA/+EAGEV4aWYAAElJKgAIAAAAAAAAAAAAAAD/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAEEAQQDAREAAhEBAxEB/8QAHQABAAMAAwEBAQAAAAAAAAAAAAYHCAEDBAUCCf/EAEcQAQABAwMABgUIBQcNAAAAAAABAgMEBQYRBwgSITFRE0FWlNEXGCI0c5Gy0hQVMlVhNVRykpOxsxYjJTM2N0NTYnF0gYL/xAAbAQEAAgMBAQAAAAAAAAAAAAAABQYDBAcBAv/EADkRAQABAgMDCQcCBQUAAAAAAAABAgMEBREGITESFjJBUVJTcdEUFWGBkaHBE7EjMzVysiI0kuHw/9oADAMBAAIRAxEAPwDKgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJBtzZu4NyY13I0PSsjNs2q/R112ojimrjnjvnyaGLzTCYKqKMRcimZ372SizXc30xq+v8lW9/ZzO+6n4tTnFlnjR9/Rk9lu90+Sre/s5nfdT8TnFlnjR9/Q9lu90+Sre/s5nfdT8TnFlnjR9/Q9lu90+Sre/s5nfdT8TnFlnjR9/Q9lu90+Sre/s5nfdT8TnFlnjR9/Q9lu90+Sre/s5nfdT8TnFlnjR9/Q9lu90+Sre/s5nfdT8TnFlnjR9/Q9lu91CaqZpqmmqOJieJhMxOu+Gu4egAAAAAAAAAAAAAAAAAAAAAADTnVX/wBkNY/8+P8ADhzTbb/d2/7fzKZy3oT5rr4jyUtJHEeQHEeQHEeQHEeQHEeQHEeQP5/Zf1q9/Tq/vd8t9GFVni6X28AAAAAAAAAAAAAAAAAAAAAcx4wDaPRDjWJ6MtuTNizMziRMzNumZn6VX8HGs+uV+8b2+el2/CFgwlMTZp3Jnbt0W4mLdFFET38U0xH9yGmqauMtuIiOD9PAAAAAAB0zi4/83sf2dPwff6lfbP1fPIp7GPeneimjpW16mimmmmKrXEUxxH+qode2ZmZyy1M/H/KUBjI0vVIAnmqAAAAAAAAAAAAAAAAAAA9GBbpvZuPbuRzRXcppmP4TMQ+LtU00TMdj2N8tgT0O7EiqYjQo7p/nN78zkPOjNPF+1Pon4wNnu/umej6biaPpmNp2nWvQ4eNR6O1b7U1dmny5nmZ8fWh8RfuYm5VeuzrVVvls0UU0RyaeD2ML6AAAAAAAAQ/XujXaWvarf1LVtJjIzb8xNy56e5T2uIiI7oqiPCIS+Gz7H4S1Fmzc0pjhGkejXrwlq5Vyqo3qG6wu0dE2pm6Jb0DBjEoyLV2q7HpK6+1MVUxH7Uz5r3spmeJx9F2cTVytJjTdEdU9iLx1mi1MRRGioVtaAAAAAAAAAAAAAAAAACS9HW2qN3bvwdFuZNWLRk9uZu00duaezRVV4cx5cI3NsfOX4SvExTytNN3DjMQzWLX6tcUa8V4YHV7wMXOxsivX8i7RauU3Krc4sRFcRMTxzFfdzxwpN3bW7coqoi1EaxO/X/pJU5bETryl5zPMzPnPKkJNwAAAAAAAAAACPbo2Xt/dl/Fr3Bp8ZdWPE0Wpm7XR2YqmJn9mY58ISGCzXF5fTVGGr5OvHdE8POGG7h7d3fXGrEWpW6LOoZNu3HZoou100x5RFUxDtdmqardNU9cQrdW6XmZHgAAAAAAAAAAAAAACUbI2PrO9LmZRoduxXOLTTVd9Ldi3x2pmI458fCUZmWbYbLYpnETP+rXTSNeDNZsV3tYpW10TdFO59s7903VtVs4lOHYi525t5NNdX0rdVMd0fxmFTz3aPBY7A12LMzyp06pjhMS3sLhLlu7FVXBoVz9LgAAAAAAAAAE+AKK6QemrVdsby1TRsbStPv2cS5FFNy5VciqqJpie/iePWvOVbKWMdhKMRVcqiao4Rp2+SKvY+u3XNMRwR6OsTrUTE/qTS+6ef2rv5khzIw3i1fb0YveVzshSeVenIybt6qIiblc1zEeEczyulFPIpimOpHzOs6up9PAAAAAAAAAAAAAAAF/dU/63ub7LH/FWoW3PQsedX4SmWdKr5NEuepcAAAAAAAAABXvSJ0p6bsbWLGn5+BmZNy9Yi/FdiqiIiJqmnjvnx+isGU7PXs0tTet1xEROm/Xsify1MRi6bFXJmEVnrD6Dx/I2qf17fxSnMnFeJT92D3lR2SofpD16xufeeqazi2rlmzl3Irpt3JiaqYimI7+O71L1lWDqwOEt4eudZpjq80XeuRcrmqOtHEixAAAAAAAAAAAAAAAAAL+6p/1vc32WP+KtQtuehY86vwlMs6VXyaJc9S4AAAAAAAD4e6t16LtTHx72v5v6Jav1zRbq9HXX2piOZj6MTx3N7A5bicfVNOGp5Uxx3xH7sV2/Ra6co38sWxP37Hut78qR5r5p4X3p9WH26x3lDdP25tI3TuvBzNBy/wBKx7eFTaqr9HVRxVFdc8cVRE+Ewvey+X4jAYWq3iKeTM1a8YndpHYi8bdou1xNE9SsVlaYAAAAAAAAAAAAAAAAAAC/uqf9b3N9lj/irULbnoWPOr8JTLOlV8miXPUuAAAAAA6crJsYdiq9l37VizTxE3LtcUUxz4d89z7ot13KuTREzPw3vKqopjWXh/yi0T986Z75b/Mz+w4nwqv+M+j4/Wo7YUr1ntTwM/QdDpwc7Eyaqcm7NUWL9NyYjsR48TPC57GYe7av3ZuUTG6OMTHX8UbmNdNUU6Szs6EigAAAAAAHfh2f0jLs2Zq7MXK6aOfLmYgGrvml4Ptbk+40/nBlvcenRpG4NT02m5N2nDyruPFyY4mrsVzTzx6ueAfOAAAAAAAAAABf3VP+t7m+yx/xVqFtz0LHnV+EplnSq+TRLnqXAAAAAAVx1hf91Gq/a2P8WlY9lP6pb8qv2lpY/wDkyyB3/wAHW0CT/wCno4AAAAAAABsbqdaRpuf0b6jeztPw8i9TqtyKa71imuqI9FamIiZjzBo4FEdbDSNNxuifMysbT8O1k1ZtiartuxTTXMzVPPNURz3gxEAAAAAAAAAAC/uqf9b3N9lj/irULbnoWPOr8JTLOlV8miXPUuAAAAAA6snHsZVmbOVZtX7U8TNF2iK6Z48O6e590V1W55VE6T8NzyqmKo0l4v1Do/7o033S3+Vm9sxHiVfWfV8fo2+yFKdaDTsLC0HQ6sLCxceqrJuxVNmzTRMx2I8eIhc9jL927fuxcqmd0cZmetG5jRTTFPJhnV0NFAAAAAAAJfsrpA3LtWi3haLreXp+n15EXrtu1MRTMz2YmZ7vKI+4G3flv6OfarD/ALO5+UGM+krpH3DubUdY0/I17KztCrzrlzHs1THYmiLlU25ju58OOAV+AAAAAAAAAAC/uqf9b3N9lj/irULbnoWPOr8JTLOlV8miXPUuAAAAAAAA+Hurami7rx8ezr+F+l2rFc126fSV0dmZjiZ+jMc9zewWY4nAVTVhquTM8d0T+7Fds0XenGqOfI9sT9xR7ze/Mkec+aeL9qfRi9hsd391C9P22dI2tuvBw9BxIxMe5hU3aqIuVV81TXXHPNUzPhEL1svj8Rj8LVcxFXKmKtOERu0jsRWNtUWq4iiNNysVmaYAAAAADnmfMHAAAAAAAAAAAAL+6p/1vc32WP8AirULbnoWPOr8JTLOlV8miXPUuAAAAAAAAAAr7pD6LdL3zq9jUNRzs7Hu2bEWIpsRR2ZiKpq574nv+lKfynaG/ldqbNqiJiZ136+XV5NTEYOm/VyplFp6vO3uP5X1b7rf5Upz2xfh0/f1YPdlHbKhekTQbG2N56po+Jdu3rGJciiiu7x2piaYnv47vWveU4yrHYO3iK40mqOrzRd63FuuaY6kcSLEAAAAAAAAAAAAAAAAAsnoZ6QcPYV7Vq87ByMuMyi1TT6Gumns9mapnnn+krm0OS3M1ptxbqink68fjo28LiYsTMzGuq7NkdMumbt3NiaNi6Vm497Iivs3LlyiaY7NM1d8R3+pS8y2XvZfhqsTXciYjTdET1zokbOOi7XFGnFaSrt8AAAAAAAAAAnwBRXSB0K6pufeOqazj6rgWLOXcium3cprmqmIpiO/iOPUvOVbV2MDhKMPVbmZpjq07UXewFdyuaonij8dXbWZmI/Xemd88fsXPg3+e+G8Kr7erD7tr7VJZVmcfJu2apiZt1zRMx6+J4XWirl0xVHWj5jSdHU+ngAAAAAAAAAAAAAAACb9DGrYOh9I2l6hq2TRi4dqLvbu1xMxTzaqiPCJnxmELtDhruKy+5as061Tpu+cNjCV00XYqq4NOW+lHZVyumijcOJVVVMUxEUXO+Z/+XM52ezKI1mzP29U17ZZ7yaT3TwhmyAAAAAAAAAAA+HuLdug7ZvY1GvanZwqr/NVuLlNU9qImOfCJ829hMtxWNiqcPRNWnHh+WG7fotbq5Ye1O5Td1HKuW6u1RVdrqpmPXE1S7ZZpmm3TE9kK3VxeVleAAAAAAAAAAAAAAAAAPXpcz+ssXv/AOLR+KGK9/Lq8p/Z7Txhv2qY7dXfHjPrcFjgtMS4HoAAAAAAAAB3ecBqzn1rp/0ltzie/wBBe9f/AF0uibDx/DvecftKHzLpUqEXtGAAAAAAAAAAAAAAAAAAAO2L93mP85X/AFpfPIp7HurafRDPPRjtuZnv/RI/FU4zn39Rvf3fiFhwf8mlL0Q2QAAAAAADkGO+ni7cp6V9eimuqI7VruiZ/wCVQ6/sxTE5Xa1jt/ylXsbP8ar/AN1K+rrqrnmuqav+88p+IiODVfl6AAAAAAAAAAAAAAAAAAAAJnpPSdvDSNNxtP07WrljDx6OxatxZtz2afLmaefWh7+QZfiLlV27a1qnjOs+rPRibtEcmmdzQXV+3PrG6dt6lk69m1Zl+1lxborqopp4p7ETx9GI9agbVZfh8DiKKMNTyYmnXr7filsBdru0zNc6rSVdvAAAAAAMdZPS7vqjIuU07guxTFUxEegtef8ARdfo2ZyuaYmbMfWfVXpxl7XpIZrmr52u6rf1LVsirJzb8xNy7VERNXEREd0REeEQmcNhrWFtRZsxpTHCGvXXNc8qri8DO+QAAAAAAAAAAAAAAAAAAAAAFkdGPSnkbD0nLwbGl2M2nIvxfmu5dqomn6MU8d0fwVzOdnqM1u03ark06Rpw169W3h8VNiJiITH5xed7O4fvNfwQ/Me140/SGx7yq7p84vO9ncP3mv4HMe140/SD3lV3T5xed7O4fvNfwOY9rxp+kHvKrunzi872dw/ea/gcx7XjT9IPeVXdPnF53s7h+81/A5j2vGn6Qe8qu6fOLzvZ3D95r+BzHteNP0g95Vd0+cXnerbuH7zX8HvMe140/SD3lV2KJu1zcuVVzHE1TMrzEaRojH4egAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAD//ZDQo="
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)

@timing
def post_ObjectRecognition(my_access_token):
    print("==============")
    print("post_ObjectRecognition...")
    chat_url = host + "ObjectRecognition/"
    headers = {
        "Authorization": "Bearer " + my_access_token,
        "Cache-Control": "no-cache",
        "Content-Type": "application/x-www-form-urlencoded"
    }
    payload = {
        "img": "data:image/jpg;base64,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"
    }
    res = requests.post(
        url=chat_url,
        data=payload,
        headers=headers
    )
    response = res.content
    print(response)



if __name__ == "__main__":
    # test token
    token_dict = get_token()
    access_token = token_dict["access_token"]
    my_post(access_token)

    # test bot
    yoyoBot = post_bot_get_handle(access_token)
    chat_with_bot(msg="推荐电影", yoyoBot_str=yoyoBot, my_access_token=access_token)

    # test cut
    post_exactCut(my_access_token=access_token)
    post_searchCut(my_access_token=access_token)
    post_kwAnalyse(my_access_token=access_token)
    post_wordFlag(my_access_token=access_token)
    post_sentiments(my_access_token=access_token)
    post_similar(my_access_token=access_token)
    post_abstract(my_access_token=access_token)
    post_content(my_access_token=access_token)
    post_classification(my_access_token=access_token)

    # test img
    post_FaceRecognition(my_access_token=access_token)
    post_ImageClassification(my_access_token=access_token)
    post_ObjectRecognition(my_access_token=access_token)

```