## JSON解析之 getJSONObject 与 optJSONObject 的区别
```

二者功能类似，返回值不同
//optJSONObject源码解析：     
     /**
     * Returns the value mapped by {@code name} if it exists and is a {@code
     * JSONObject}. Returns null otherwise.
     */
    public JSONObject optJSONObject(String name) {
        Object object = opt(name);
        return object instanceof JSONObject ? (JSONObject) object : null;
    }
    //当返回值不是JSONObject对象时，返回值为null，不抛出异常;
 
 
//getJSONObject源码解析：
     /**
     * Returns the value mapped by {@code name} if it exists and is a {@code
     * JSONObject}.
     * @throws JSONException if the mapping doesn't exist or is not a {@code
     * JSONObject}.
     */
    public JSONObject getJSONObject(String name) throws JSONException {
        Object object = get(name);
        if (object instanceof JSONObject) {
            return (JSONObject) object;
        } else {
            throw JSON.typeMismatch(name, object, "JSONObject");
        }
    }
    //当返回值不是JSONObject对象时，抛出异常;
```