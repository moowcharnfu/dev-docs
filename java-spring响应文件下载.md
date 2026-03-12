```

springmvc response, 前端需要octet-stream"
...
response.setHeader("Access-Control-Expose-Headers","Content-Disposition");
response.setContentType("application/octet-stream");
response.setCharacterEncoding("utf-8");
            fileName = String.format(fileName, duty, type, DateUtil.getCurrentLocalDateTime(DateUtil.YYYYMMDDHHMISS));
            fileName = URLEncoder.encode(fileName, "UTF-8").replaceAll("\\+", "%20");
 response.setHeader("Content-disposition", "attachment;filename*=UTF-8''" + fileName);
...

```
