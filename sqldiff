import pymssql #引入pymssql模块
def dbinit(sqlurl,name,pwd,database):
    connect = pymssql.connect(sqlurl, name, pwd,database)
    if connect:
        print("连接成功!")
    else:
        return print("失败!")
    return connect
 

def gettables(connect): 
    cursor = connect.cursor()
    sql = '''
        SELECT Name FROM SysObjects Where XType='U' ORDER BY Name
            '''
    try: 
        cursor.execute(sql) 
        results = cursor.fetchall() 
        return results
    except Exception as e:
        raise e
    # finally:
    #     connect.close()
   
def gettablediff(_oldres,_newres):
    hasdiff=False
    for row in _oldres: 
        name = row[0] 
        rowtype=row[1]
        typediff=True
        namediff=True
        for newrow in _newres: 
            if(newrow[0]==name):
                namediff=False
                if(newrow[1]==rowtype):
                    typediff=False
                    break
        
        if(namediff):
            hasdiff=True
            print(" 字段不存在: "+name)
        elif(typediff):
            hasdiff=True
            print(" 类型不匹配: "+name+",type: "+rowtype)   
    return hasdiff

def gettableinfosql(table_name):
    return '''
        SELECT 
        syscolumns.name '字段',systypes.name '类型',syscolumns.length '长度' 
        FROM syscolumns 
        JOIN systypes ON syscolumns.xusertype = systypes.xusertype
        WHERE syscolumns.id = object_id('%s') 
        '''%table_name
def gettableinfo(connect,table_name):
    cursor = connect.cursor()
    # print(gettableinfosql(table_name))
    cursor.execute(gettableinfosql(table_name)) 
    results = cursor.fetchall()
    return results
    # for i in range(len(results)):
    #     row=results[i]
    #     name = row[0] 
    #     _type=row[1] 
    #     print(table_name+ " name: "+name+",type: "+_type)
'''
    db1 主要数据库
    db2 对比数据库（不一致）
'''
def diffdb(db1,db2):
    for row in  gettables(db1):
        name=row[0]
       
        has= gettablediff(gettableinfo(db1,name),gettableinfo(db2,name))  
        if(has):
             print(name) 


if __name__ == '__main__': 
    db = dbinit("数据库地址","数据账号","密码","库名") 
    db2=dbinit("对比数据库地址","数据账号","密码","库名") 
    diffdb(db,db2)

  
