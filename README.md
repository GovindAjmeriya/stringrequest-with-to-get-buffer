# stringrequest-with-to-get-buffer
stringrequest with to get buffer
 //string request get data
                StringRequest stringRequest=new StringRequest(Request.Method.GET, url, new Response.Listener<String>() {
                    @Override
                    public void onResponse(String response)
                    {
                        Log.e("RRRRRRRresponse", "onErrorResponse: "+response );


                        try {
                            JSONObject jsonobj = new JSONObject(response);

                            if (jsonobj.getString("status").toLowerCase().equals("error")) {
                                Log.e("error", "onResponse: " + "ERROR");

                            } else if (jsonobj.getString("status").toLowerCase().equals("success")) {

                                JSONArray ary=jsonobj.getJSONObject("buffer").getJSONArray("data");


                                File file = new File(Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS),compnyName+jsonobj.getString("fileName")+".xlsx");
                                file.createNewFile();

                                BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file));
                                byte[] b;
                                String s;
                                for (int i = 0; i < ary.length(); i++)
                                {
                                  
                                    b = ary.get(i).toString().getBytes();
                                     s = new String(b, "UTF-8");
                                    bos.write(Integer.parseInt(s));
                                    
                                   //simplyfay
                                    bos.write(Integer.parseInt(new String(ary.get(i).toString().getBytes(), "UTF-8")));

                                }
                                bos.flush();
                                bos.close();
                            }

                            } catch (JSONException e) {
                            e.printStackTrace();
                        } catch (FileNotFoundException e) {e.printStackTrace();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }

                    }

                }, new Response.ErrorListener() {
                    @Override
                    public void onErrorResponse(VolleyError error) {

                        Log.e("error", "onErrorResponse: "+error );

                    }
                }){

                    @Override
                    public Map<String, String> getHeaders() throws AuthFailureError {
                        Map<String, String> params = new HashMap<String, String>();

                        params.put("Authorization", authToken);
                        Log.e("TAG", "getParams: " + params);
                        return params;
                    }
                };

                RequestQueue requestQueue = Volley.newRequestQueue(SalaryAdvice.this);
                requestQueue.add(stringRequest);
                Log.d("TAG", "sendRequest: " + stringRequest);
            }

        });
