# splunk-queries
index="id_2" | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  | rex field=_raw "LZEDP (?<LZEDP>.*)" | table AUDIT_TS LZEDP 
| join type=inner AUDIT_TS [search index="id_2" | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  | rex field=_raw "LZKAFKA (?<LZKAFKA>.*)" | table AUDIT_TS LZKAFKA]


WORING as requirement 
index="id_2" LZEDP | rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval edp = "EDP"| rex field=_raw "LZEDP (?<LZEDP>.*)" | table AUDIT_ID edp | join type=left AUDIT_ID [ search index="id_2" LZKAFKA | rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval kafka = "KAFKA"| table AUDIT_ID kafka ]


index="id_2" LZEDP | rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval edp = "EDP"| rex field=_raw "LZEDP (?<LZEDP>.*)" | table AUDIT_ID edp | join type=left AUDIT_ID [ search index="id_2" LZKAFKA | rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval kafka = "KAFKA"| table AUDIT_ID kafka ]



Simple table creation query without join 

index="id_2" LZKAFKA | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval EDP = "LZKAFKA"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID EDP | join type=left AUDIT_ID [ search 
index="id_2" LZEDP  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval EDP = "LZEDP "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID EDP ]


Final query :

index="id_2" LZKAFKA | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{37})" | eval LZKAFKA = "LZ_KAFKA"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZKAFKA | join type=left AUDIT_ID [ search index="id_2" LZEDP  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{37})" | eval LZEDP = "LZ_EDP "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZEDP ]




index="id_2" LZKAFKA | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZKAFKA = "LZ_KAFKA"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZKAFKA | join type=left AUDIT_ID [ search 
index="id_2" LZEDP  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZEDP = "LZ_EDP "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZEDP ]

==================================================================================================================

Used QUERY for EDP - LZKAFKA

index="id_2" LZEDP | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZEDP = "EDP"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZEDP | join type=left AUDIT_ID [ search index="id_2" LZKAFKA  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZKAFKA = "LZ_KAFKA "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZKAFKA ]


==================================================================================================================

Used QUERY for EDP - SLKAFKA

index="id_2" LZEDP | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZEDP = "EDP"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZEDP | join type=left AUDIT_ID [ search index="id_2" SLKAFKA  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval SLKAFKA = "SL_KAFKA "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID SLKAFKA ]

==================================================================================================================

Used QUERY for EDP - CAMUS

index="id_2" LZEDP | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZEDP = "EDP"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZEDP | join type=left AUDIT_ID [ search index="id_2" CAMUS  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval CAMUS = "CAMUS "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID CAMUS ]


==================================================================================================================

Used QUERY for EDP - LZKAFKA - SLKAFKA - CAMUS

index="id_2" LZEDP | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZEDP = "EDP"| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZEDP | join type=left AUDIT_ID [ search index="id_2" LZKAFKA  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval LZKAFKA = "LZ_KAFKA "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID LZKAFKA | join type=left AUDIT_ID [ search index="id_2" SLKAFKA  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval SLKAFKA = "SL_KAFKA "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID SLKAFKA  | join type=left AUDIT_ID [ search index="id_2" CAMUS  | rex field=_raw "AUDIT_TS:(?<AUDIT_TS>.{10})"  |rex field=_raw "AUDIT_ID:(?<AUDIT_ID>.{36})" | eval CAMUS = "CAMUS "| rex field=_raw "SOURCE:(?<SOURCE>[^\s]+)" | rex field=_raw "TABLE:(?<TABLE>[^\s]+)"  | table AUDIT_TS SOURCE TABLE AUDIT_ID CAMUS] ] ]
