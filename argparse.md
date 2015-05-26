    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument("did", type=int, help="did of which device to be used")
    parser.add_argument("-l", "--loops", type=int, default=1, help="run tests multiple times; -1 = infinity")
    parser.add_argument("-w", "--wait", type=int, default=5, help="seconds to wait between loops; default is 5")
    parser.add_argument("-p", "--params", type=json.loads,
                        help="append additional parameters passed (json); e.g. '{\"key1\":\"value1\"}'")
    cmd_args = parser.parse_args()
    klass.did = utils.get_params_in_list(cmd_args.did)
    if cmd_args.params:
        klass.params = cmd_args.params
