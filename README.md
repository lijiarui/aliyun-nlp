# ALIYUN-NLP
test aliyun api

Doesn't support restful api yet.

## BASIC JSON FORMAT
INPUT: 帮我订一张北京去上海的飞机票，下周二的
```json
{
  "version":"3.0",
  "status":"1",
  "frames":
  [                                                                           
  {                                                                           
    "domain":"flight_ticket",                                               
    "intent":"search_flight_ticket",                                    
    "score":1.0,                                                        
    "slots":{                                                           
      "airtype":[],                                                   
      "company":[],                                                   
      "flight_number":[],                                             
      "from_airport":[],                                              
      "from_geo":[                                                    
      {                                                               
        "level_1":{                                                 
            "norm":"",                                              
            "raw":""                                                
        },                                                          
        "level_2":{                                                 
            "norm":"",                                              
            "raw":""                                                
        },                                                          
        "level_3":{                                                 
            "norm":"北京",                                          
            "raw":"北京"                                            
        },                                                          
        "level_4":{                                                 
            "norm":"",                                              
            "raw":""                                                
        },                                                          
        "level_5":{                                                 
            "norm":"",                                              
            "raw":""                                                
        },                                                          
        "location":[]                                               
      }                                                               
      ],                                                              
      "grade":[],                                                 
      "price":[],                                                 
      "sort":[],                                                  
      "time":[                                                    
      {                                                           
        "error_code":"0",                                       
        "norm":[                                                
          "2015-12-08 00:00:00",                              
          "2015-12-08 23:59:59"                                   
        ],                                                  
        "raw":"下周二",                                         
        "relative_mode":"0",                                    
        "type":"interval"                                       
      }                                                           
      ],                                                              
      "to_ariport":[],                                            
      "to_geo":[                                                  
      {                                                           
        "level_1":{                                             
          "norm":"",                                          
          "raw":""                                            
        },                                                      
        "level_2":{                                             
          "norm":"",                                          
          "raw":""                                            
        },                                                      
        "level_3":{                                             
          "norm":"上海",                                      
          "raw":"上海"                                        
        },                                                      
        "level_4":{                                             
          "norm":"",                                          
          "raw":""                                            
        },                                                      
        "level_5":{                                             
          "norm":"",                                          
          "raw":""                                            
        },                                                      
        "location":[]                                           
      }                                                           
      ]                                                               
    }                                                                   
  }                                                                           
  ]
}
```

## Example
[酒店](https://help.aliyun.com/document_detail/30454.html?spm=5176.doc30442.2.10.Ngij3y)
[飞机票](https://help.aliyun.com/document_detail/30451.html?spm=5176.doc30442.2.17.Ngij3y)
[火车票](https://help.aliyun.com/document_detail/30450.html?spm=5176.doc30442.2.16.Ngij3y)
[MORE](https://help.aliyun.com/document_detail/30442.html?spm=5176.doc30441.6.570.snaOtE)

## SLOT TYPE

### BASIC
```json
"tv_channel":[
  {
    "norm":"中央一套",
    "raw":"cctv1"
  }
]
```

### DATE

#### TYPE
日期时间类型定义了一种结构，可以描述时间点和时间段两种类型，根据type字段的取值不同，type我们定义了5种类型，less，greater，equal，around，interval。

| type类型   | 时间点/时间区间 | 说明      |
| -------- | -------- | ------- |
| less     | 时间区间     | 某个时间点之前 |
| greater  | 时间区间     | 某个时间点之后 |
| equal    | 时间点      | 某个时间点   |
| around   | 时间区间     | 某个时间点左右 |
| interval | 时间区间     | 两个时间点之间 |

#### PROTOCOL

在时间段的表述中，有早上，下午等时间段表述。我们定义这些时间段的始末时间为：

| 字段名称                         | 字段类型   | 字段说明                |
| ---------------------------- | ------ | ------------------- |
| type                         | string | 上表中定义的5种取值          |
| raw                          | string | query中的原始描述         |
| norm                         | array  | 归一化后的时间描述           |
| error_code                   | string | 0正常， 1超长范围错误, 2未知错误 |
| relative_mode                | string | 1相对时间, 0绝对时间        |
| 注：norm是一个数组，其中的每个元素都是一个日期时间， |        |                     |
| 格式为：yyyy-mm-dd hh:mm:ss      |        |                     |

#### MORNING, NOON, NIGHT

| 时间段表述    | 开始    | 结束    | 时间点   |
| -------- | ----- | ----- | ----- |
| 早晨，早上，清晨 | 06:00 | 08:00 | 07:00 |
| 上午       | 08:00 | 12:00 | 10:00 |
| 中午、晌午    | 11:00 | 13:00 | 12:00 |
| 下午、午后    | 12:00 | 18:00 | 15:00 |
| 晚上，傍晚    | 18:00 | 24:00 | 21:00 |
| 凌晨       | 00:00 | 05:00 | 03:00 |
| 半夜、深夜、午夜 | 23:00 | 01:00 | 00:00 |

#### EXAMPLE

1. query: 明天上午九点
```json
"time":[
  {
    "error_code":"0",
    "norm":[
      "2015-12-02 09:00:00"
    ],
    "raw":"明天上午九点",
    "relative_mode":"0",
    "type":"equal"
  }
]
```

1. query: 明天上午八点之前
```json
```

1. query: 明天上午九点
```json
"time":[
  {
    "error_code":"0",
    "norm":[
      "2015-12-02 08:00:00"
    ],
    "raw":"明天上午八点之前",
    "relative_mode":"0",
    "type":"less"
  }
]
```

1. query: 明天下午五点之后
```json
"time":[
  {
    "error_code":"0",
    "norm":[
      "2015-12-02 17:00:00"
    ],
    "raw":"明天下午五点之后",
    "relative_mode":"0",
    "type":"greater"
  }
]
```

1. query: 明天上午七点左右
```json
"time":[
  {
    "error_code":"0",
    "norm":[
      "2015-12-02 07:00:00"
    ],
    "raw":"明天上午七点左右",
    "relative_mode":"0",
    "type":"around"
  }
]
```

1. query: 明天上午
```json
"time":[
  {
    "error_code":"0",
    "norm":[
      "2015-12-02 08:00:00",
      "2015-12-02 12:00:00"
    ],
    "raw":"明天上午",
    "relative_mode":"0",
    "type":"interval"
  }
]
```

### GEO_INFO
GEO_INFO定义了一个表示地点的数据结构，地点协议的描述：

| 字段名称                                     | 字段类型  | 字段说明 |
| ---------------------------------------- | ----- | ---- |
| level_1                                  | BASIC | 国家   |
| level_2                                  | BASIC | 省    |
| level_3                                  | BASIC | 市    |
| level_4                                  | BASIC | 区县   |
| level_5                                  | BASIC | 乡镇村  |
| location                                 | array | POI点 |
| 备注：location中的<br>shape表示该location是一个点还是一条线，<br>tag表示该location具体的语义标签 |       |      |

#### EXAMPLE
1. query: 中国浙江省杭州市余杭区文三路附近的ktv，其中的地点geo结构为
```json
"geo":[
  {
    "level_1":{
      "norm":"中国",
      "raw":"中国"
    },
    "level_2":{
      "norm":"浙江省",
      "raw":"浙江省"
    },
    "level_3":{
      "norm":"杭州市",
      "raw":"杭州市"
    },
    "level_4":{
      "norm":"余杭区",
      "raw":"余杭区"
    },
    "level_5":{
      "norm":"",
      "raw":""
    },
    "location":[
      {
        "norm":"文三路",
        "raw":"文三路",
        "shape":"line",
        "tag":[
          "道路"
        ]
      }
    ]
  }
]
```

### NUMBER
数字用途也很广泛，例如价格，折扣，距离等属性都可以用数字这个结构来表示。 数字的协议定义：

| 字段名称 | 字段类型   | 字段说明               |
| ---- | ------ | ------------------ |
| type | string | 取值范围同DATETIME的type |
| raw  | string | query中的原始描述        |
| norm | array  | 归一化后的数字描述          |

#### EXAMPLE
1. query: 1000元左右
```json
"price":[
  {
    "norm":[
      1000
    ],
    "raw":"1000元左右",
    "type":"around"
  }
]
```

1. query: 1000到2000元
```json
"price":[
  {
    "norm":[
      1000,
      2000
    ],
    "raw":"1000到2000元",
    "type":"interval"
  }
]
```

1. query: 500米以内
```json
"radius":[
  {
    "norm":[
      500
    ],
    "raw":"500米以内",
    "type":"less"
  }
]
```
