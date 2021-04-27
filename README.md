# Benchmarks of Go serialization methods

[![Gitter chat](https://badges.gitter.im/alecthomas.png)](https://gitter.im/alecthomas/Lobby)

This is a test suite for benchmarking various Go serialization methods.

## Tested serialization methods

- [encoding/gob](http://golang.org/pkg/encoding/gob/)
- [encoding/json](http://golang.org/pkg/encoding/json/)
- [github.com/json-iterator/go](https://github.com/json-iterator/go)
- [github.com/alecthomas/binary](https://github.com/alecthomas/binary)
- [github.com/davecgh/go-xdr/xdr](https://github.com/davecgh/go-xdr)
- [github.com/Sereal/Sereal/Go/sereal](https://github.com/Sereal/Sereal)
- [github.com/ugorji/go/codec](https://github.com/ugorji/go/tree/master/codec)
- [github.com/vmihailenco/msgpack/v4](https://github.com/vmihailenco/msgpack)
- [labix.org/v2/mgo/bson](https://labix.org/v2/mgo/bson)
- [github.com/tinylib/msgp](https://github.com/tinylib/msgp) *(code generator for msgpack)*
- [github.com/golang/protobuf](https://github.com/golang/protobuf) (generated code)
- [github.com/gogo/protobuf](https://github.com/gogo/protobuf) (generated code, optimized version of `goprotobuf`)
- [go.dedis.ch/protobuf](https://go.dedis.ch/protobuf) (reflection based)
- [github.com/google/flatbuffers](https://github.com/google/flatbuffers)
- [github.com/hprose/hprose-go/io](https://github.com/hprose/hprose-go)
- [github.com/glycerine/go-capnproto](https://github.com/glycerine/go-capnproto)
- [zombiezen.com/go/capnproto2](https://godoc.org/zombiezen.com/go/capnproto2)
- [github.com/andyleap/gencode](https://github.com/andyleap/gencode)
- [github.com/pascaldekloe/colfer](https://github.com/pascaldekloe/colfer)
- [github.com/linkedin/goavro](https://github.com/linkedin/goavro)
- [github.com/ikkerens/ikeapack](https://github.com/ikkerens/ikeapack)
- [github.com/niubaoshu/gotiny](https://github.com/niubaoshu/gotiny)
- [github.com/prysmaticlabs/go-ssz](https://github.com/prysmaticlabs/go-ssz)
- [github.com/200sc/bebop](https://github.com/200sc/bebop) (generated code)
- [github.com/shamaton/msgpackgen](https://github.com/shamaton/msgpackgen) (generated code)

## Running the benchmarks

```bash
go get -u -t
go test -bench='.*' ./
```

To update the table in the README:

```bash
./stats.sh
```

## Recommendation

If performance, correctness and interoperability are the most
important factors, [gogoprotobuf](https://gogo.github.io/) is
currently the best choice. It does require a pre-processing step (eg.
via Go 1.4's "go generate" command).

But as always, make your own choice based on your requirements.

## Data

The data being serialized is the following structure with randomly generated values:

```go
type A struct {
    Name     string
    BirthDay time.Time
    Phone    string
    Siblings int
    Spouse   bool
    Money    float64
}
```


## Results

2021-4-26 Results with Go 1.16.3 darwin/amd64 on a windows/amd64 on a 3.4 GHz Intel Core i7 16GB

benchmark                                     | iter       | time/iter    | bytes/op  | allocs/op  | tt.sec | tt.kb        | ns/alloc
----------------------------------------------|------------|--------------|-----------|------------|--------|--------------|-----------
Gotiny_Marshal-8                    |    8080758 |    145 ns/op |        48 |          0 |   1.17 |        38787 |    0.00
Gotiny_Unmarshal-8                  |    4560700 |    257 ns/op |        47 |        112 |   1.17 |        21886 |    2.30
GotinyNoTime_Marshal-8              |    9835911 |    123 ns/op |        48 |          0 |   1.21 |        47212 |    0.00
GotinyNoTime_Unmarshal-8            |    5031410 |    240 ns/op |        48 |         96 |   1.21 |        24150 |    2.51
Msgp_Marshal-8                      |    6873883 |    171 ns/op |        97 |        128 |   1.18 |        66676 |    1.34
Msgp_Unmarshal-8                    |    3841492 |    306 ns/op |        97 |        112 |   1.18 |        37262 |    2.74
VmihailencoMsgpack_Marshal-8        |    1000000 |   1131 ns/op |       100 |        392 |   1.13 |        10000 |    2.89
VmihailencoMsgpack_Unmarshal-8      |     750735 |   1663 ns/op |       100 |        408 |   1.25 |         7507 |    4.08
Json_Marshal-8                      |     566748 |   2211 ns/op |       149 |        208 |   1.25 |         8489 |   10.63
Json_Unmarshal-8                    |     269678 |   4452 ns/op |       149 |        383 |   1.20 |         4037 |   11.62
JsonIter_Marshal-8                  |     545456 |   2265 ns/op |       139 |        248 |   1.24 |         7603 |    9.13
JsonIter_Unmarshal-8                |     705882 |   1800 ns/op |       139 |        264 |   1.27 |         9818 |    6.82
EasyJson_Marshal-8                  |     750135 |   1706 ns/op |       149 |        896 |   1.28 |        11237 |    1.90
EasyJson_Unmarshal-8                |     857240 |   1525 ns/op |       149 |        160 |   1.31 |        12832 |    9.53
Bson_Marshal-8                      |    1000000 |   1129 ns/op |       110 |        376 |   1.13 |        11000 |    3.00
Bson_Unmarshal-8                    |     749919 |   1649 ns/op |       110 |        224 |   1.24 |         8249 |    7.36
Gob_Marshal-8                       |    1613995 |    763 ns/op |        63 |         40 |   1.23 |        10265 |   19.09
Gob_Unmarshal-8                     |    1481431 |    802 ns/op |        63 |        112 |   1.19 |         9410 |    7.17
XDR_Marshal-8                       |     705998 |   1803 ns/op |        92 |        440 |   1.27 |         6495 |    4.10
XDR_Unmarshal-8                     |     857106 |   1523 ns/op |        92 |        232 |   1.31 |         7885 |    6.56
UgorjiCodecMsgpack_Marshal-8        |     922998 |   1323 ns/op |        91 |       1304 |   1.22 |         8399 |    1.01
UgorjiCodecMsgpack_Unmarshal-8      |     959907 |   1267 ns/op |        91 |        496 |   1.22 |         8735 |    2.55
UgorjiCodecBinc_Marshal-8           |     888882 |   1423 ns/op |        95 |       1320 |   1.26 |         8444 |    1.08
UgorjiCodecBinc_Unmarshal-8         |     727069 |   1549 ns/op |        95 |        656 |   1.13 |         6907 |    2.36
Sereal_Marshal-8                    |     413800 |   2912 ns/op |       132 |        832 |   1.20 |         5462 |    3.50
Sereal_Unmarshal-8                  |     362913 |   3272 ns/op |       132 |        976 |   1.19 |         4790 |    3.35
Binary_Marshal-8                    |     959516 |   1311 ns/op |        61 |        312 |   1.26 |         5853 |    4.20
Binary_Unmarshal-8                  |     827505 |   1461 ns/op |        61 |        320 |   1.21 |         5047 |    4.57
FlatBuffers_Marshal-8               |    4669252 |    259 ns/op |        95 |          0 |   1.21 |        44502 |    0.00
FlatBuffers_Unmarshal-8             |    4647493 |    258 ns/op |        95 |        112 |   1.20 |        44155 |    2.31
CapNProto_Marshal-8                 |    3127926 |    392 ns/op |        96 |         56 |   1.23 |        30028 |    7.00
CapNProto_Unmarshal-8               |    2746615 |    429 ns/op |        96 |        192 |   1.18 |        26367 |    2.24
CapNProto2_Marshal-8                |    1929469 |    618 ns/op |        96 |        244 |   1.19 |        18522 |    2.54
CapNProto2_Unmarshal-8              |    1557546 |    780 ns/op |        96 |        304 |   1.22 |        14952 |    2.57
Hprose_Marshal-8                    |    1000000 |   1015 ns/op |        84 |        441 |   1.01 |         8478 |    2.30
Hprose_Unmarshal-8                  |    1000000 |   1140 ns/op |        85 |        304 |   1.14 |         8529 |    3.75
Hprose2_Marshal-8                   |    2230474 |    539 ns/op |        85 |          0 |   1.20 |        19019 |    0.00
Hprose2_Unmarshal-8                 |    2179842 |    553 ns/op |        85 |        136 |   1.21 |        18587 |    4.07
Protobuf_Marshal-8                  |    1641498 |    724 ns/op |        52 |        144 |   1.19 |         8535 |    5.03
Protobuf_Unmarshal-8                |    1658491 |    752 ns/op |        52 |        184 |   1.25 |         8624 |    4.09
Goprotobuf_Marshal-8                |    1305718 |    928 ns/op |        53 |         96 |   1.21 |         6920 |    9.68
Goprotobuf_Unmarshal-8              |    1627296 |    733 ns/op |        53 |        184 |   1.19 |         8624 |    3.99
Gogoprotobuf_Marshal-8              |    8791292 |    133 ns/op |        53 |         64 |   1.18 |        46593 |    2.09
Gogoprotobuf_Unmarshal-8            |    5297970 |    229 ns/op |        53 |         96 |   1.21 |        28079 |    2.39
Colfer_Marshal-8                    |    9954499 |    119 ns/op |        51 |         64 |   1.19 |        50857 |    1.87
Colfer_Unmarshal-8                  |    6282636 |    197 ns/op |        51 |        112 |   1.24 |        32041 |    1.76
Gencode_Marshal-8                   |    6843366 |    175 ns/op |        53 |         80 |   1.20 |        36269 |    2.19
Gencode_Unmarshal-8                 |    5712628 |    212 ns/op |        53 |        112 |   1.21 |        30276 |    1.89
GencodeUnsafe_Marshal-8             |   12500038 |     96 ns/op |        46 |         48 |   1.20 |        57500 |    2.01
GencodeUnsafe_Unmarshal-8           |    7451287 |    157 ns/op |        46 |         96 |   1.18 |        34275 |    1.64
XDR2_Marshal-8                      |    7767140 |    156 ns/op |        60 |         64 |   1.22 |        46602 |    2.45
XDR2_Unmarshal-8                    |    8727132 |    131 ns/op |        60 |         32 |   1.15 |        52362 |    4.11
GoAvro_Marshal-8                    |     452804 |   2652 ns/op |        47 |        872 |   1.20 |         2128 |    3.04
GoAvro_Unmarshal-8                  |     173912 |   6756 ns/op |        47 |       3024 |   1.17 |          817 |    2.23
GoAvro2Text_Marshal-8               |     374949 |   2976 ns/op |       133 |       1320 |   1.12 |         5013 |    2.25
GoAvro2Text_Unmarshal-8             |     413770 |   2846 ns/op |       133 |        783 |   1.18 |         5532 |    3.63
GoAvro2Binary_Marshal-8             |    1305085 |    914 ns/op |        47 |        464 |   1.19 |         6133 |    1.97
GoAvro2Binary_Unmarshal-8           |    1000000 |   1084 ns/op |        47 |        544 |   1.08 |         4700 |    1.99
Ikea_Marshal-8                      |    1790031 |    663 ns/op |        55 |         72 |   1.19 |         9845 |    9.22
Ikea_Unmarshal-8                    |    1412598 |    815 ns/op |        55 |        160 |   1.15 |         7769 |    5.10
ShamatonMapMsgpack_Marshal-8        |    1634973 |    738 ns/op |        92 |        192 |   1.21 |        15041 |    3.85
ShamatonMapMsgpack_Unmarshal-8      |    1531588 |    777 ns/op |        92 |        136 |   1.19 |        14090 |    5.72
ShamatonArrayMsgpack_Marshal-8      |    1833718 |    662 ns/op |        50 |        160 |   1.21 |         9168 |    4.14
ShamatonArrayMsgpack_Unmarshal-8    |    2205799 |    556 ns/op |        50 |        136 |   1.23 |        11028 |    4.09
ShamatonMapMsgpackgen_Marshal-8     |    5620184 |    209 ns/op |        92 |         96 |   1.18 |        51705 |    2.18
ShamatonMapMsgpackgen_Unmarshal-8   |    5306834 |    225 ns/op |        92 |         80 |   1.20 |        48822 |    2.82
ShamatonArrayMsgpackgen_Marshal-8   |    7716738 |    162 ns/op |        50 |         64 |   1.25 |        38583 |    2.54
ShamatonArrayMsgpackgen_Unmarshal-8 |    7473162 |    162 ns/op |        50 |         80 |   1.22 |        37365 |    2.03
SSZNoTimeNoStringNoFloatA_Marshal-8 |     260875 |   4554 ns/op |        55 |        440 |   1.19 |         1434 |   10.35
SSZNoTimeNoStringNoFloatA_Unmarshal-8 |     159942 |   7659 ns/op |        55 |       1184 |   1.22 |          879 |    6.47
Mum_Marshal-8                       |   12435219 |     96 ns/op |        48 |          0 |   1.21 |        59689 |    0.00
Mum_Unmarshal-8                     |    5593518 |    211 ns/op |        48 |         80 |   1.18 |        26848 |    2.64
Bebop_Marshal-8                     |   18182092 |     66 ns/op |        55 |          0 |   1.22 |       100001 |    0.00
Bebop_Unmarshal-8                   |   27273160 |     44 ns/op |        55 |          1 |   1.21 |       150002 |   44.22
BebopMessage_Marshal-8              |   14907405 |     81 ns/op |        65 |          0 |   1.21 |        96898 |    0.00
BebopMessage_Unmarshal-8            |    6045306 |    201 ns/op |        66 |         56 |   1.22 |        39899 |    3.60


Totals:


benchmark                                | iter       | time/iter    | bytes/op  | allocs/op  | tt.sec | tt.kb        | ns/alloc
-----------------------------------------|------------|--------------|-----------|------------|--------|--------------|-----------
Bebop-8                       |   45455252 |    111 ns/op |       110 |          1 |   5.06 |       500007 |  111.21
GencodeUnsafe-8               |   19951325 |    254 ns/op |        92 |        144 |   5.07 |       183552 |    1.77
BebopMessage-8                |   20952711 |    283 ns/op |       131 |         56 |   5.93 |       274480 |    5.05
XDR2-8                        |   16494272 |    288 ns/op |       120 |         96 |   4.76 |       197931 |    3.01
Mum-8                         |   18028737 |    307 ns/op |        96 |         80 |   5.55 |       173075 |    3.85
Colfer-8                      |   16237135 |    317 ns/op |       102 |        176 |   5.15 |       165764 |    1.80
ShamatonArrayMsgpackgen-8     |   15189900 |    324 ns/op |       100 |        144 |   4.94 |       151899 |    2.26
Gogoprotobuf-8                |   14089262 |    363 ns/op |       106 |        160 |   5.12 |       149346 |    2.27
GotinyNoTime-8                |   14867321 |    363 ns/op |        96 |         96 |   5.41 |       142726 |    3.79
Gencode-8                     |   12555994 |    387 ns/op |       106 |        192 |   4.86 |       133093 |    2.02
Gotiny-8                      |   12641458 |    402 ns/op |        95 |        112 |   5.08 |       121345 |    3.59
ShamatonMapMsgpackgen-8       |   10927018 |    435 ns/op |       184 |        176 |   4.76 |       201057 |    2.47
Msgp-8                        |   10715375 |    477 ns/op |       194 |        240 |   5.12 |       207878 |    1.99
FlatBuffers-8                 |    9316745 |    517 ns/op |       190 |        112 |   4.83 |       177316 |    4.62
CapNProto-8                   |    5874541 |    821 ns/op |       192 |        248 |   4.83 |       112791 |    3.31
Hprose2-8                     |    4410316 |   1092 ns/op |       170 |        136 |   4.82 |        75213 |    8.04
ShamatonArrayMsgpack-8        |    4039517 |   1218 ns/op |       100 |        296 |   4.92 |        40395 |    4.12
CapNProto2-8                  |    3487015 |   1399 ns/op |       192 |        548 |   4.88 |        66950 |    2.55
Protobuf-8                    |    3299989 |   1476 ns/op |       104 |        328 |   4.87 |        34319 |    4.50
Ikea-8                        |    3202629 |   1479 ns/op |       110 |        232 |   4.74 |        35228 |    6.38
ShamatonMapMsgpack-8          |    3166561 |   1516 ns/op |       184 |        328 |   4.80 |        58264 |    4.62
Gob-8                         |    3095426 |   1566 ns/op |       127 |        152 |   4.85 |        39349 |   10.31
Goprotobuf-8                  |    2933014 |   1662 ns/op |       106 |        280 |   4.88 |        31089 |    5.94
GoAvro2Binary-8               |    2305085 |   1998 ns/op |        94 |       1008 |   4.61 |        21667 |    1.98
Hprose-8                      |    2000000 |   2155 ns/op |       170 |        745 |   4.31 |        34014 |    2.89
UgorjiCodecMsgpack-8          |    1882905 |   2590 ns/op |       182 |       1800 |   4.88 |        34268 |    1.44
Binary-8                      |    1787021 |   2772 ns/op |       122 |        632 |   4.95 |        21801 |    4.39
Bson-8                        |    1749919 |   2778 ns/op |       220 |        600 |   4.86 |        38498 |    4.63
VmihailencoMsgpack-8          |    1750735 |   2794 ns/op |       200 |        800 |   4.89 |        35014 |    3.49
UgorjiCodecBinc-8             |    1615951 |   2972 ns/op |       190 |       1976 |   4.80 |        30703 |    1.50
EasyJson-8                    |    1607375 |   3231 ns/op |       299 |       1056 |   5.19 |        48140 |    3.06
XDR-8                         |    1563104 |   3326 ns/op |       184 |        672 |   5.20 |        28761 |    4.95
JsonIter-8                    |    1251338 |   4065 ns/op |       278 |        512 |   5.09 |        34849 |    7.94
GoAvro2Text-8                 |     788719 |   5822 ns/op |       267 |       2103 |   4.59 |        21090 |    2.77
Sereal-8                      |     776713 |   6184 ns/op |       264 |       1808 |   4.80 |        20505 |    3.42
Json-8                        |     836426 |   6663 ns/op |       299 |        591 |   5.57 |        25050 |   11.27
GoAvro-8                      |     626716 |   9408 ns/op |        94 |       3896 |   5.90 |         5891 |    2.41
SSZNoTimeNoStringNoFloatA-8   |     420817 |  12213 ns/op |       110 |       1624 |   5.14 |         4628 |    7.52





## Issues


The benchmarks can also be run with validation enabled.

```bash
VALIDATE=1 go test -bench='.*' ./
```

Unfortunately, several of the serializers exhibit issues:

1. **(minor)** BSON drops sub-microsecond precision from `time.Time`.
3. **(minor)** Ugorji Binc Codec drops the timezone name (eg. "EST" -> "-0500") from `time.Time`.

```
--- FAIL: BenchmarkBsonUnmarshal-8
    serialization_benchmarks_test.go:115: unmarshaled object differed:
        &{20b999e3621bd773 2016-01-19 14:05:02.469416459 -0800 PST f017c8e9de 4 true 0.20887343719329818}
        &{20b999e3621bd773 2016-01-19 14:05:02.469 -0800 PST f017c8e9de 4 true 0.20887343719329818}
--- FAIL: BenchmarkUgorjiCodecBincUnmarshal-8
    serialization_benchmarks_test.go:115: unmarshaled object differed:
        &{20a1757ced6b488e 2016-01-19 14:05:15.69474534 -0800 PST 71f3bf4233 0 false 0.8712180830484527}
        &{20a1757ced6b488e 2016-01-19 14:05:15.69474534 -0800 -0800 71f3bf4233 0 false 0.8712180830484527}
```

All other fields are correct however.

Additionally, while not a correctness issue, FlatBuffers, ProtoBuffers, Cap'N'Proto and ikeapack do not
support time types directly. In the benchmarks an int64 value is used to hold a UnixNano timestamp.
