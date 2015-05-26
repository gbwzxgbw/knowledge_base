    # Another implementation, first choice
    def attempt_conn(ip, port, account=None):
        import os
        os.sys.path.insert(0, os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__)))))
        from framework import app_client, accounts
    
        try:
            print "try connecting to {}:{}".format(ip, port)
            transport, protocol, client = app_client.create_thrift_connection(ip, port)
        except:
            print "connected to {}:{} fail".format(ip, port)
            # print e
            return None
        else:
            # use an invalid account to test connectivity
            if not account:
                account = accounts.account_invalid
            username = account['username']
            password = account['password']
            result = client.remote_api_login(1, "miio", username, password)
    
            # release the connection
            print "try to release {}:{}".format(ip, port)
            if None != transport:
                transport.close()
                transport = None
    
            return ip, port
    
    
    def get_available_server_2():
        from multiprocessing import Pool, TimeoutError
        import time
        import random
    
        # WARNING: Try invoking serviceLoginAuth2 multiple times may trigger
        #          the policy from by xiaomi.com, and it will lead to
        #          all ip banned (since we have limited public ip address)
        #          which is a critical issue.
        #
        TRIES_COUNT = 0
        TRIES_COUNT_2 = 1
        MAX_TRIES_COUNT = 3
    
        available_conn_list = []
        candidate_conn_list = []
    
        for conn in SERVER_LISTENER:
            ip = conn['ip']
            port_base = conn['port']
            for offset in range(0, conn['offset']):
                port = port_base + offset
                candidate_conn_list.append((ip, port))
    
        while candidate_conn_list and not available_conn_list:
            count = 0
            TRIES_COUNT, TRIES_COUNT_2 = (TRIES_COUNT_2 if TRIES_COUNT_2 < MAX_TRIES_COUNT else MAX_TRIES_COUNT,
                                          TRIES_COUNT + TRIES_COUNT_2)
            pool = Pool(TRIES_COUNT)
            async_result_list = []
            while candidate_conn_list and count < TRIES_COUNT:
                index = random.randrange(0, len(candidate_conn_list))
                async_result_list.append(pool.apply_async(attempt_conn, candidate_conn_list[index]))
                candidate_conn_list.pop(index)
                count += 1
    
            time.sleep(3)  # leave enough time to avoid exceeding login limit
            for res in async_result_list:
                try:
                    host = res.get(timeout=0)
                except TimeoutError:
                    pass
                else:
                    if host:
                        available_conn_list.append(host)
            pool.terminate()
        pass
    
        ip, host = available_conn_list[0] if available_conn_list else (None, None)
        print ip, host
        return ip, host
