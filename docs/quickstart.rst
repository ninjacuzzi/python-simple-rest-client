Quickstart
==========

Let's start building a client for users resource in https://reqres.in/ service::
    
    >>> # import API
    >>> from simple_rest_client.api import API
    >>> # create api instance
    >>> api = API(
    ...     api_root_url='https://reqres.in/api/', # base api url
    ...     params={}, # default params
    ...     headers={}, # default headers
    ...     timeout=2, # default timeout in seconds
    ...     append_slash=False, # append slash to final url
    ...     json_encode_body=True, # encode body as json
    ... )
    >>> # add users resource
    >>> api.add_resource(resource_name='users')
    >>> # show resource actions
    >>> api.users.actions
    {'list': {'method': 'GET', 'url': 'users'}, 'create': {'method': 'POST', 'url': 'users'}, 'retrieve': {'method': 'GET', 'url': 'users/{}'}, 'update': {'method': 'PUT', 'url': 'users/{}'}, 'partial_update': {'method': 'PATCH', 'url': 'users/{}'}, 'destroy': {'method': 'DELETE', 'url': 'users/{}'}} 
    >>> # list action
    >>> response = api.users.list(body=None, params={}, headers={})
    >>> response.url
    'https://reqres.in/api/users'
    >>> response.method
    'GET'
    >>> response.body
    {'page': 1, 'per_page': 3, 'total': 12, 'total_pages': 4, 'data': [{'id': 1, 'first_name': 'george', 'last_name': 'bluth', 'avatar': 'https://s3.amazonaws.com/uifaces/faces/twitter/calebogden/128.jpg'}, {'id': 2, 'first_name': 'lucille', 'last_name': 'bluth', 'avatar': 'https://s3.amazonaws.com/uifaces/faces/twitter/josephstein/128.jpg'}, {'id': 3, 'first_name': 'oscar', 'last_name': 'bluth', 'avatar': 'https://s3.amazonaws.com/uifaces/faces/twitter/olegpogodaev/128.jpg'}]}
    >>> response.headers
    {'Date': 'Sat, 15 Apr 2017 21:39:46 GMT', 'Content-Type': 'application/json; charset=utf-8', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'X-Powered-By': 'Express', 'Access-Control-Allow-Origin': '*', 'ETag': 'W/"1be-q96WkDv6JqfLvIPiRhzWJQ"', 'Server': 'cloudflare-nginx', 'CF-RAY': '35020f33aaf04a9c-GRU', 'Content-Encoding': 'gzip'}
    >>> response.status_code
    200
    >>> # create action
    >>> body = {'name': 'morpheus', 'job': 'leader'}
    >>> response = api.users.create(body=body, params={}, headers={})
    >>> response.status_code
    201
    >>> # retrieve action
    >>> response = api.users.retrieve(2, body=None, params={}, headers={})
    >>> response.status_code
    200
    >>> # update action
    >>> response = api.users.update(2, body=body, params={}, headers={})
    >>> response.status_code
    200
    >>> # partial update action
    >>> response = api.users.partial_update(2, body=body, params={}, headers={})
    >>> response.status_code
    200
    >>> # destroy action
    >>> response = api.users.destroy(2, body=None, params={}, headers={})
    >>> response.status_code
    204

Now, building a client for github events resource (https://developer.github.com/v3/activity/events/)::

    >>> # import API and Resource
    >>> from simple_rest_client.api import API
    >>> from simple_rest_client.resource import Resource
    >>> # create EventResource with custom actions
    >>> class EventResource(Resource):
    ...     actions = {
    ...         'public_events': {'method': 'GET', 'url': 'events'},
    ...         'repository_events': {'method': 'GET', 'url': '/repos/{}/{}/events'},
    ...         'repository_issues_events': {'method': 'GET', 'url': '/repos/{}/{}/issues/events'},
    ...         'public_network_events': {'method': 'GET', 'url': '/networks/{}/{}/events'},
    ...         'public_organization_events': {'method': 'GET', 'url': '/orgs/{}/events'},
    ...         'user_received_events': {'method': 'GET', 'url': '/users/{}/received_events'},
    ...         'public_user_received_events': {'method': 'GET', 'url': '/users/{}/received_events/public'},
    ...         'user_events': {'method': 'GET', 'url': '/users/{}/events'},
    ...         'public_user_events': {'method': 'GET', 'url': '/users/{}/events/public'},
    ...         'organization_events': {'method': 'GET', 'url': '/users/{}/events/orgs/{}'},
    ... }
    ... 
    >>> # set default params
    >>> default_params = {'access_token': 'valid-token'}
    >>> # create api instance
    >>> github_api = API(
    ...     api_root_url='https://api.github.com', params=default_params,
    ...     json_encode_body=True
    ... )
    >>> # add events resource with EventResource
    >>> github_api.add_resource(resource_name='events', resource_class=EventResource)
    >>> # show resource actions
    >>> github_api.events.actions
    {'public_events': {'method': 'GET', 'url': 'events'}, 'repository_events': {'method': 'GET', 'url': '/repos/{}/{}/events'}, 'repository_issues_events': {'method': 'GET', 'url': '/repos/{}/{}/issues/events'}, 'public_network_events': {'method': 'GET', 'url': '/networks/{}/{}/events'}, 'public_organization_events': {'method': 'GET', 'url': '/orgs/{}/events'}, 'user_received_events': {'method': 'GET', 'url': '/users/{}/received_events'}, 'public_user_received_events': {'method': 'GET', 'url': '/users/{}/received_events/public'}, 'user_events': {'method': 'GET', 'url': '/users/{}/events'}, 'public_user_events': {'method': 'GET', 'url': '/users/{}/events/public'}, 'organization_events': {'method': 'GET', 'url': '/users/{}/events/orgs/{}'}}
    >>> # public_events action
    >>> response = github_api.events.public_events(body=None, params={}, headers={})
    >>> response.url
    'https://api.github.com/events?access_token=valid-token'
    >>> response.method
    'GET'
    >>> # repository_events action
    >>> response = github_api.events.repository_events('allisson', 'python-simple-rest-client', body=None, params={}, headers={})
    >>> response.url
    'https://api.github.com/repos/allisson/python-simple-rest-client/events?access_token=valid-token'
    >>> response.method
    'GET'

Check `https://github.com/allisson/python-simple-rest-client/tree/master/examples <https://github.com/allisson/python-simple-rest-client/tree/master/examples>`_ for more code examples.