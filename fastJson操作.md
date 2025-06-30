1. 在使用FastJson进行数据序列化时，可能会遇到"$ref"循环引用检测的问题

String jsonString = JSON.toJSONString(object, SerializerFeature.DisableCircularReferenceDetect);
