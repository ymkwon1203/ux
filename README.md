* :runner: 현재 실장 대상
* :heavy\_check\_mark: Backend실장완료(테스트는 아직 못함)
* :white\_check\_mark: Backend실장완료(테스트 완료)
* :ballot\_box\_with\_check: Frontend까지 실장 완료
* :x: 사용하지 않는 API

# 목차
[[_TOC_]]

# /api/resources
## :white\_check\_mark: ✅ 1. GET /api/resources/new/step2/settings/:func_id @각 기능의 기본 설정값 요청
front <<< back

**local 타입**

```json
{
    "source_type": "local",
    "form": [
        {
            "title": "1. Set Display Tab Name",
            "items": [
                {
                    "target": "tab_name",
                    "title": "Tab Name",
                    "type": "input",
                    "content": ""
                }
            ]
        },
        {
            "title": "2. Select Source Files",
            "items": [
                {
                    "target": "src_file",
                    "title": "Log Files",
                    "type": "files",
                    "enable": true,
                    "file_name": []
                }
            ]
        },
        {
            "title": "3. Log Type",
            "items": [
                {
                    "target": "log_name",
                    "title": "Log Name",
                    "type": "input",
                    "content": "TiltMeasurementLog"
                }
            ]
        }
    ]    
}
```

**remote 타입**

```json
{
    "source_type": "remote",
    "form": [
        {
            "title": "1. Set Display Tab Name",
            "items": [
                {
                    "target": "tab_name",
                    "title": "Tab Name",
                    "type": "input",
                    "content": ""
                }
            ]
        },
        {
            "title": "2. Select Data Source",
            "items": [
                {
                    "target": "db_id",
                    "title": "From",
                    "type": "select",
                    "mode": "singular",
                    "options": [
                        {
                            "id": 2,
                            "name": "cras_db@10.1.31.230"
                        },
                        {
                            "id": 3,
                            "name": "cras_db@10.1.31.230"
                        }
                    ],
                    "selected": 2
                },
                {
                    "target": "table_name",
                    "title": "",
                    "type": "input",
                    "content": "adc_measurement"
                }
            ]
        },
        {
            "title": "3. Select Equipment",
            "items": [
                {
                    "target": "user_fab",
                    "title": "User/Fab",
                    "type": "select",
                    "mode": "subItem",
                    "options": [
                        "BSOT/FAB1",
                        "TED/Fab1"
                    ],
                    "selected": "BSOT/FAB1",
                    "subItem": {
                        "target": "equipment_name",
                        "title": "Equipment",
                        "type": "select",
                        "mode": "singular",
                        "options": [
                            {
                                "BSOT/FAB1": [
                                    "BSOT_FAB1_equipment1",
                                    "BSOT_FAB1_equipment2"
                                ]
                            },
                            {
                                "TED/Fab1": [
                                    "TED_Fab1_MPA_2"
                                ]
                            }
                        ],
                        "selected": "BSOT_FAB1_equipment1"
                    }
                }
            ]
        },
        {
            "title": "4. Select Start/End Date",
            "items": [
                {
                    "target": "period",
                    "title": "Period",
                    "type": "DatePicker",
                    "period": {
                        "start": "2019-05-21 12:25:35",
                        "end": "2019-05-21 13:37:23"
                    },
                    "selected": {
                        "start": "2019-05-21",
                        "end": "2019-05-21"
                    },
                    "enable": true
                }
            ]
        }
    ]
}
```

**sql 타입**

```json
{
    "source_type": "sql",
    "form": [
        {
            "title": "1. Set Display Tab Name",
            "items": [
                {
                    "target": "tab_name",
                    "title": "Tab Name",
                    "type": "input",
                    "content": ""
                }
            ]
        },
        {
            "title": "2. Select Data Source",
            "items": [
                {
                    "target": "db_id",
                    "title": "From",
                    "type": "select",
                    "mode": "singular",
                    "options": [
                        {
                            "id": 2,
                            "name": "cras_db@10.1.31.230"
                        },
                        {
                            "id": 3,
                            "name": "cras_db@10.1.31.230"
                        }
                    ],
                    "selected": 2
                }
            ]
        },
        {
            "title": "3. Input SQL",
            "items": [
                {
                    "target": "sql",
                    "title": "",
                    "type": "textarea",
                    "content": "select * from adc_measurement",
                    "enable": false
                }
            ]
        }
    ]
}
```

## :white\_check\_mark: 2. POST /api/preview/samplelog/multi  @Step2 Multi Preview 요청

Preview실시 시, Single Function 분석 시의 시퀀스를 유용

**2-1. Local Function의 경우**

1. [POST /api/converter/file](Rest_API#-post-apiconverterfile-분석로그파일-업로드)  @분석로그파일 업로드
2. [POST /api/converter/job](Rest_API#-post-apiconverterjob-변환-요청)  @변환 요청
3. [GET /api/converter/job/:rid](Rest_API#-get-apiconverterjobrid-변환-처리-상태-확인)  @변환 처리 상태 확인

front <<< back
rid 반환

**2-2. Remote Function의 경우**

1. [GET /api/analysis/remote/:func_id](Rest_API#-get-apianalysisremotefunc_id-remote타입의-로그-취득)  @Remote타입의 로그 취득

front <<< back
rid 반환

**2-3. SQL Function의 경우**
1. [GET /api/analysis/sql/:func_id](Rest_API#-get-apianalysissqlfunc_id-sql타입의-로그-취득)  @SQL타입의 로그 취득

front <<< back
rid 반환

**2-4. POST /api/preview/samplelog/multi  @Preview 요청**

front >>> back
```json
{
    "use_org_analysis": false, //true/false
    "func_list": [
        {
            "func_id": 1,
            "tab_name": "tab1",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 2,
            "tab_name": "tab2",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 3,
            "tab_name": "tab3",
            "rid" : "request_xxxxxx"
        }
    ]
}
```

**use_org_analysis가 false일때**

front <<< back

```json
{
    "result": true,
    "data": {
        "tab1": {
            "disp_order": [
                "log_time"
            ],
            "src_col_list": [],
            "row": {
                "1": {
                    "log_time": "2019-05-21 12:25:35"
                }
            }
        },
        "tab2": {
            "disp_order": [
                "log_time"
            ],
            "src_col_list": [],
            "row": {
                "1": {
                    "log_time": "2019-05-21 12:25:35"
                }
            }
        }
    },
    "analysis": {
        "type": "setting", //setting(default), script, none
        "setting": {
            "calc_type": [
                "max",
                "min",
                "ave",
                "sum",
                "3sigma",
                "sequential",
                "script"
            ],
            "aggregation_type": [
                "period",
                "column",
                "all"
            ],
    	    "items": [],
	    "filter_default": [],
	    "aggregation_default": {
	        "type": "column",
		"val": null
	    },
            "column_script_default": "from resource.script.analysis_base import AnalysisBase ..."
        },
        "script": {
            "data_source": [
                 {
                     "id": 1,
                     "name": ""
                 },
                 {
                     "id": 2,
                     "name": ""
                 }
            ],
            "db_id": null,
            "sql": "",
            "file_name": null,
            "use_script": false
        },
        "filter_key_list": []
    },
    "visualization": {
        "function_graph_type": [
            {
                "id": null,
                "name": "default",
                "script": "",
                "type": "system"
            }
        ],
        "graph_list": [
            {
                "name": "Bar",
                "type": "system"
            },
            {
                "name": "Line",
                "type": "system"
            },
            {
                "name": "Box Plot",
                "type": "system"
            },
            {
                "name": "Density Plot",
                "type": "system"
            },
            {
                "name": "Bubble Chart",
                "type": "system"
            }
        ],
        "items": []
    }
}
```

**use_org_analysis가 true일때**

front <<< back

```json
{
    "data": {
        "tab_name1": {
            "disp_order": [],
            "disp_graph": [],
            "row": {
                "1": {
                }
            }
        },
        "tab_name2": {
            "disp_order": [],
            "disp_graph": [],
            "row": {
                "1": {
                }
            }
        }   
    },
    "visualization": {
        "function_graph_type": [
            {
                "id": null,
                "name": "default",
                "script": "",
                "type": "system"
            }
        ],
        "graph_list": [
            {
                "name": "Bar",
                "type": "system"
            },
            {
                "name": "Line",
                "type": "system"
            },
            {
                "name": "Box Plot",
                "type": "system"
            },
            {
                "name": "Density Plot",
                "type": "system"
            },
            {
                "name": "Bubble Chart",
                "type": "system"
            }
        ],
        "items": [],
        "common_axis_x": [],  // 그래프의 X축에 공통으로 사용할 수 있는 컬럼 리스트
    }
}
```

## :white\_check\_mark: 3. POST /api/preview/analysis/multi  @멀티로그분석의 Analysis Preview요청

**Setting**

front >>> back

- json_data: 아래 json포맷을 blob을 이용해서 업로드
- script_file: 스크립트 파일 업로드. 없을 경우 제외
- use_script: false
- func_id: 편집모드 preview시 기존 등록되어있는  스크립트를 사용하기 위해 필요

```json
{
    "func_list": [
        {
            "func_id": 1,
            "tab_name": "tab1",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 2,
            "tab_name": "tab2",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 3,
            "tab_name": "tab3",
            "rid" : "request_xxxxxx"
        }
    ],
    "analysis": {
        "type": "setting",  //setting, script, none
        "items": [
            {
                "disp_order": "1",
                "source_col": ["tab1/c", "tab2/d"],
                "title": "max cd",
                "group_analysis": "delta.abs.max",
                "group_analysis_type": "sequential",
                "total_analysis": "max",
                "total_analysis_type": "max"
            },
            {
                "disp_order": "2",
                "source_col": ["tab1/c", "tab2/d"],
                "title": "BLT Sum",
                "group_analysis": "from resource.script.analysis_base import AnalysisBase\nimport numpy as np\n\n\nclass ColumnAnalysisScript(AnalysisBase):\n    \"\"\"\n    .. class:: ColumnAnalysisScript\n\n        This class is for analyze specific column data by user script.\n    \"\"\"\n    def __init__(self, **kwargs):\n        super().__init__(**kwargs)\n\n    def run(self, data) -> float:\n        \"\"\"\n        This method will be called by Main Process. Fix this method as you want.\n\n        :param data: pandas.Series Data\n        :return: analyze result. (float)\n        \"\"\"\n\n        return np.nanmax(data.values)\n",
                "group_analysis_type": "script",
                "total_analysis": "max",
                "total_analysis_type": "max"
            }
        ],
        "filter_default": [
            {
                "key": "a",
                "val": [1]
            }
        ],
        "aggregation_default": {
            "type": "period",
            "val": "1h"
        }
    }
}
```

front <<< back
```json
{
    "data": {
        "disp_order": [],
        "disp_graph": [],
        "row": {
            "1": {
            }
        }
    }
}
```

**Script**

front >>> back

- json_data: 아래 json포맷을 blob을 이용해서 업로드
- script_file: 스크립트 파일 업로드. 없을경우 제외
- use_script: true, false

```json
{
    "func_list": [
        {
            "func_id": 1,
            "tab_name": "tab1",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 2,
            "tab_name": "tab2",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 3,
            "tab_name": "tab3",
            "rid" : "request_xxxxxx"
        }
    ],
    "analysis": {
        "type": script,  //setting, script, none
        "script": {
            "db_id": 1,
            "sql": "select * from table"
        }
    }
}
```

front <<< back
```json
{
    "data": {
        "disp_order": [],
        "disp_graph": [],
        "row": {
            "1": {
            }
        }
    }
}
```

**None**

front >>> back

- json_data: 아래 json포맷을 blob을 이용해서 업로드

```json
{
    "func_list": [
        {
            "func_id": 1,
            "tab_name": "tab1",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 2,
            "tab_name": "tab2",
            "rid" : "request_xxxxxx"
        },
        {
            "func_id": 3,
            "tab_name": "tab3",
            "rid" : "request_xxxxxx"
        }
    ],
    "analysis": {
        "type": none,  //setting, script, none        
        "filter_default": [
            {
                "key": "deflection",
                "val": ["*", "a"]
            }
        ]        
    }
}
```

front <<< back
```json
{
    "data": [
        {
           "tab_name1": {
                "disp_order": [],
                "disp_graph": [],
                "row": {
                    "1": {
                    }
                }
        },
        {
            "tab_name2": {
                "disp_order": [],
                "disp_graph": [],
                "row": {
                    "1": {
                    }
                }
            }   
        }
    ],
    "visualization": {
        "common_axis_x": [],  // 그래프의 X축에 공통으로 사용할 수 있는 컬럼 리스트
    }
}
```

## :white\_check\_mark: 4. GET /api/resources/new @ 신규등록 시

**use_org_analysis False인 경우**

front >>> back
```json
{
    "func": {
        "category_id": 1,
        "title": "multi",
        "source_type": "multi",
        "info": [
            {
                "func_id": 26,
                "tab_name": "tab1",
                "fid": [50],
                "rid": "request_20211224_084135352762"
            },
            {
                "func_id": 27,
                "tab_name": "tab2",
                "rid": "request_20211224_084135352762",
                "db_id": 2,
                "table_name": "adc_measurement",
                "equipment_name": "BSOT_FAB1_equipment1",
                "period_start": "2019-05-21",
                "period_end": "2019-05-21"
            },
            {
                "func_id": 28,
                "tab_name": "tab3",
                "rid": "request_20211224_084135352762",
                "db_id": 2,
                "sql": "select * from adc_compensation"
            }
        ], 
        "use_org_analysis": false
    },
    "convert": {},
    "filter": {},
    "analysis": {
        "type": "setting",
        "setting": {
            "items": [
                {
                    "disp_order": "1",
                    "source_col": ["tab1/c", "tab2/logicalposition_x"],
                    "title": "max",
                    "group_analysis": "delta.abs.max",
                    "group_analysis_type": "sequential",
                    "total_analysis": "max",
                    "total_analysis_type": "max"
                },
                {
                    "disp_order": "2",
                    "source_col": ["tab1/c", "tab3/measuredoffset_arc_u_drive"],
                    "title": "BLT Sum",
                    "group_analysis": "from resource.script.analysis_base import AnalysisBase\nimport numpy as np\n\n\nclass ColumnAnalysisScript(AnalysisBase):\n    \"\"\"\n    .. class:: ColumnAnalysisScript\n\n        This class is for analyze specific column data by user script.\n    \"\"\"\n    def __init__(self, **kwargs):\n        super().__init__(**kwargs)\n\n    def run(self, data) -> float:\n        \"\"\"\n        This method will be called by Main Process. Fix this method as you want.\n\n        :param data: pandas.Series Data\n        :return: analyze result. (float)\n        \"\"\"\n\n        return np.nansum(data.values)\n",
                    "group_analysis_type": "script",
                    "total_analysis": "max",
                    "total_analysis_type": "max"
                }
            ],
            "filter_default": [
                {
                    "key": "plate",
                    "val": [1,2,3,4,5]
                }
            ],
            "aggregation_default": {
                "type": "period",
                "val": "1h"
            }
        },
        "script": {
            "db_id": 2,
            "sql": "",
            "file_name": null,
            "use_script": false
        }
    },
    "visualization": {
        "function_graph_type": [
            {
                "id": null,
                "name": "default",
                "script": "function renderGraph(Plotly, element, params) {\n    let tmpGraphProp = {};\n\n    const createPropData = (obj, type, params) => {\n        let tmpRange = {},\n            tmpDensityRange = {},\n            tmpData = [];\n\n        if (params.range.x.length > 0) {\n            tmpRange['xaxis'] = {\n                range: params.range.x,\n            };\n        }\n\n        if (params.range.y.length > 0) {\n            tmpRange['yaxis'] = {\n                range: params.range.y,\n            };\n        }\n\n        switch (type.toLowerCase()) {\n            case 'bar':\n            default:\n                Object.keys(params.y).map((v) => {\n                    tmpData.push({\n                        x: params.x,\n                        y: params.y[v],\n                        type: 'bar',\n                        name: v,\n                    });\n                });\n                if (Object.keys(obj).length === 0) {\n                    obj = {\n                        data: tmpData,\n                        layout: {\n                            title: params.title,\n                            font: {\n                                family: \"Saira, 'Nunito Sans'\",\n                            },\n                            ...tmpRange,\n                        },\n                    };\n                } else {\n                    obj = {\n                        ...obj,\n                        data: obj.data.concat(tmpData),\n                    };\n                }\n                break;\n\n            case 'line':\n                Object.keys(params.y).map((v) => {\n                    tmpData.push({\n                        x: params.x,\n                        y: params.y[v],\n                        type: 'scatter',\n                        name: v,\n                    });\n                });\n                if (Object.keys(obj).length === 0) {\n                    obj = {\n                        data: tmpData,\n                        layout: {\n                            title: params.title,\n                            font: {\n                                family: \"Saira, 'Nunito Sans'\",\n                            },\n                            ...tmpRange,\n                        },\n                    };\n                } else {\n                    obj = {\n                        ...obj,\n                        data: obj.data.concat(tmpData),\n                    };\n                }\n                break;\n\n            case 'box plot':\n                Object.keys(params.y).map((v) => {\n                    tmpData.push({\n                        y: params.y[v],\n                        type: 'box',\n                        name: v,\n                    });\n                });\n                if (Object.keys(obj).length === 0) {\n                    obj = {\n                        data: tmpData,\n                        layout: {\n                            title: params.title,\n                            font: {\n                                family: \"Saira, 'Nunito Sans'\",\n                            },\n                            ...tmpRange,\n                        },\n                    };\n                } else {\n                    obj = {\n                        ...obj,\n                        data: obj.data.concat(tmpData),\n                    };\n                }\n                break;\n\n            case 'density plot':\n                Object.keys(params.y).map((v) => {\n                    tmpData.push(\n                        {\n                            x: params.x,\n                            y: params.y[v],\n                            mode: 'markers',\n                            name: v,\n                            marker: {\n                                color: 'rgb(245,245,245)',\n                                size: 1.5,\n                                opacity: 0.7,\n                            },\n                            type: 'scatter',\n                        },\n                        {\n                            x: params.x,\n                            y: params.y[v],\n                            name: v,\n                            ncontours: 20,\n                            colorscale: 'Blues',\n                            reversescale: true,\n                            showscale: false,\n                            type: 'histogram2dcontour',\n                        },\n                    );\n                });\n\n                if (params.range.x.length > 0) {\n                    tmpDensityRange['xaxis'] = {\n                        showgrid: false,\n                        zeroline: false,\n                        range: params.range.x,\n                    };\n                }\n\n                if (params.range.y.length > 0) {\n                    tmpDensityRange['yaxis'] = {\n                        showgrid: false,\n                        zeroline: false,\n                        range: params.range.y,\n                    };\n                }\n\n                if (Object.keys(obj).length === 0) {\n                    obj = {\n                        data: tmpData,\n                        layout: {\n                            title: params.title,\n                            font: {\n                                family: \"Saira, 'Nunito Sans'\",\n                            },\n                            showlegend: false,\n                            hovermode: 'closest',\n                            bargap: 0,\n                            ...tmpDensityRange,\n                        },\n                    };\n                } else {\n                    obj = {\n                        ...obj,\n                        data: obj.data.concat(tmpData),\n                    };\n                }\n                break;\n\n            case 'bubble chart':\n                Object.keys(params.y).map((v) => {\n                    tmpData.push({\n                        x: params.x,\n                        y: params.y[v],\n                        mode: 'markers',\n                        name: v,\n                        marker: {\n                            color: 'rgb(93, 164, 214)',\n                            size: params.z,\n                        },\n                    });\n                });\n                if (Object.keys(obj).length === 0) {\n                    obj = {\n                        data: tmpData,\n                        layout: {\n                            title: params.title,\n                            font: {\n                                family: \"Saira, 'Nunito Sans'\",\n                            },\n                            ...tmpRange,\n                        },\n                    };\n                } else {\n                    obj = {\n                        ...obj,\n                        data: obj.data.concat(tmpData),\n                    };\n                }\n                break;\n        }\n        return obj;\n    };\n\n    params.type.map((v) => {\n        tmpGraphProp = createPropData(tmpGraphProp, v, params);\n    });\n\n    Plotly.newPlot(element, tmpGraphProp.data, tmpGraphProp.layout);\n\n    if (params.func) {\n        element.on('plotly_relayout', params.func);\n    }\n}\n",
                "type": "system"
            }
        ],
        "items": [
            {
                "id" : null,
                "type": [
                    "bar",
                    "line"
                ],
                "x_axis": "log_time",
                "y_axis": ["max", "BLT Sum"],
                "z_axis": null,
                "title": "test",
                "x_range_max": 1,
                "x_range_min": 0,
                "y_range_max": 1,
                "y_range_min": 0,
                "z_range_max": 1,
                "z_range_min": 0
            }
        ]
    }
}
```

**use_org_analysis True인 경우**

분석 설정 스텝을 건너뛰지만, 아래와 같이 analysis에 대한 기본 정보는 보내줘야함.

front >> back
```json
{
    "analysis": {
        "type": "setting",
        "setting": {
            "items": [
            ],
            "filter_default": [
            ],
            "aggregation_default": {
            }
        },
        "script": {
            "db_id": null,
            "sql": "",
            "file_name": null,
            "use_script": false //true, false. 파일이 선택됐으면 무조건 true, 선택이 안됐으면 무조건 False
        }
    }
}
```

## :runner: 5. GET /api/resources/main  @main페이지 리소스획득
front <<< back
```json
{
    "title": "MPA Analysis Tool",
    "navibar": [
        {
            "category_id": 1,
            "title": "test",
            "subfunc": [
                {
                    "title": "abc",
                    "func_id": 6
                }
            ]
        }
    ],
    "body": [
        {
            "category_id": 1,
            "title": "multi",
            "subfunc": [
                {
                    "title": "abc",
                    "func_id": 6,
                    "info": {
                        "Source": "local",
                        "Log Name": "PLATEAUTOFOCUSCOMPENSATION"
                    }
                }
            ]
        }
    ],
    "footer": {
        "copyright": "Copyright (c) 2021 CANON Inc. All rights reserved.",
        "version": "Ver. 1.1.0",
        "about": "About MPA Analysis Tool"
    }
}

```

## :runner: 5. GET /api/resources/settings/:func_id  @분석 기능 실행 설정 화면

**multi 타입의 경우**

front <<< back








## GET /api/resources/edit/step2/settings/:func_id @각 기능의 기본 설정값 요청(편집모드)
front <<< back
```json
{
    "title": "Tilt Meas.",
    "formList": [
        {
            "key": "local",
            "type": "local"
        }
    ],
    "form": {
        "local": [
            {
                "title": "1. Select Source",
                "items": [
                    {
                        "target": "source",
                        "title": "From",
                        "type": "select",
                        "mode": "singular",
                        "options": [
                            "local"
                        ]
                    }
                ]
            },
            {
                "title": "2. Select Source Files",
                "items": [
                    {
                        "target": "src_file",
                        "title": "Log Files",
                        "type": "files",
                        "enable": true,
                        "file_name": ["file1", "file2"]
                    }
                ]
            },
            {
                "title": "3. Select Log Type",
                "items": [
                    {
                        "target": "log_name",
                        "title": "Log Name",
                        "type": "input",
                        "content": "TiltMeasurementLog"
                    }
                ]
            }
        ]
    }
}
```



# /api/preview


## POST /api/preview/analysis/multi
### user Setting
```json
{
    "data": {
       "tab_name" : {
            "disp_order": [
            "status",
            "flt",
            "frt",
            "blt",
            "brt",
            "pitching",
            "rolling",
            "torsion",
            "deflection",
            "log_time"
        ],
        "row": {
            "1": {
                "status": 0,
                "flt": 64556,
                "frt": 70803,
                "blt": 69460,
                "brt": 65765,
                "pitching": -7,
                "rolling": -239,
                "torsion": -4,
                "deflection": "a",
                "log_time": "2021-10-01 12:45:28"
            },
            "2": {
                "status": 0,
                "flt": 62446,
                "frt": 70005,
                "blt": 71119,
                "brt": 68889,
                "pitching": 411,
                "rolling": -499,
                "torsion": -4,
                "deflection": "b",
                "log_time": "2021-10-01 18:39:33"
            },
            "3": {
                "status": 0,
                "flt": 64882,
                "frt": 71203,
                "blt": 69706,
                "brt": 65858,
                "pitching": -28,
                "rolling": -231,
                "torsion": -5,
                "deflection": "*",
                "log_time": "2021-10-01 18:39:46"
            }
        }
    },
    "analysis": {
        "type": "setting",  //setting, script, none
        "items": [
            {
                "disp_order": "1",
                "source_col": [],
                "title": "FLT Delta Max",
                "group_analysis": "delta.abs.max",
                "group_analysis_type": "sequencial",
                "total_analysis": "max",
                "total_analysis_type": "max"
            },
            {
                "disp_order": "2",
                "source_col": [],
                "title": "BLT Sum",
                "group_analysis": "data.sum()*100",
                "group_analysis_type": "custom",
                "total_analysis": "max",
                "total_analysis_type": "max"
            },
            {
                "disp_order": "3",
                "source_col": [],
                "title": "BLT2",
                "group_analysis": "sum",
                "group_analysis_type": "sum",
                "total_analysis": "max",
                "total_analysis_type": "max"
            },
            {
                "disp_order": "4",
                "source_col": [],
                "title": "BLT3",
                "group_analysis": "sum",
                "group_analysis_type": "sum",
                "total_analysis": "max",
                "total_analysis_type": "max"
            }
        ]
    }
}
```

### Script
- json_data: 아래 json포맷을 blob을 이용해서 업로드
- "script_file": //스크립트 파일 업로드. 없을경우 제외
- "use_script": true, false

```json
{
    "data": {
        "disp_order": [
            "status",
            "flt",
            "frt",
            "blt",
            "brt",
            "pitching",
            "rolling",
            "torsion",
            "deflection",
            "log_time"
        ],
        "row": {
            "1": {
                "status": 0,
                "flt": 64556,
                "frt": 70803,
                "blt": 69460,
                "brt": 65765,
                "pitching": -7,
                "rolling": -239,
                "torsion": -4,
                "deflection": "a",
                "log_time": "2021-10-01 12:45:28"
            },
            "2": {
                "status": 0,
                "flt": 62446,
                "frt": 70005,
                "blt": 71119,
                "brt": 68889,
                "pitching": 411,
                "rolling": -499,
                "torsion": -4,
                "deflection": "b",
                "log_time": "2021-10-01 18:39:33"
            },
            "3": {
                "status": 0,
                "flt": 64882,
                "frt": 71203,
                "blt": 69706,
                "brt": 65858,
                "pitching": -28,
                "rolling": -231,
                "torsion": -5,
                "deflection": "*",
                "log_time": "2021-10-01 18:39:46"
            }
        }
    },
    "analysis": {
        "type": script,  //setting, script, none
        "script": {
            "db_id": 1,
            "sql": "select * from table"
        }

    }
}
```

### none

- json_data: 아래 json포맷을 blob을 이용해서 업로드

```json
{
    "data": {
        "disp_order": [
            "status",
            "flt",
            "frt",
            "blt",
            "brt",
            "pitching",
            "rolling",
            "torsion",
            "deflection",
            "log_time"
        ],
        "row": {
            "1": {
                "status": 0,
                "flt": 64556,
                "frt": 70803,
                "blt": 69460,
                "brt": 65765,
                "pitching": -7,
                "rolling": -239,
                "torsion": -4,
                "deflection": "a",
                "log_time": "2021-10-01 12:45:28"
            },
            "2": {
                "status": 0,
                "flt": 62446,
                "frt": 70005,
                "blt": 71119,
                "brt": 68889,
                "pitching": 411,
                "rolling": -499,
                "torsion": -4,
                "deflection": "b",
                "log_time": "2021-10-01 18:39:33"
            },
            "3": {
                "status": 0,
                "flt": 64882,
                "frt": 71203,
                "blt": 69706,
                "brt": 65858,
                "pitching": -28,
                "rolling": -231,
                "torsion": -5,
                "deflection": "*",
                "log_time": "2021-10-01 18:39:46"
            }
        }
    },
    "analysis": {
        "type": none,  //setting, script, none
    }
}
```
