@Library('plx')
import plx.Application
new Application(
    [
        packageName : "plx.controllers",
        controllerName: "CiCdController",
        action: "start",
        args : [
            name: "demo",
            namespace: "innovation",
            archetype : [
                name : "php.microservice.api",
                type : "service",
                /* type : "job", */
                language : [
                    class : "Php",
                    container : [
                        builder : [
                            name : "", 
                            /*dockerfile : [
                                "name" : "",
                                "args : []
                            ],  
                            */
                            env : [:]
                        ],
                        runner : [
                             name : "", 
                             /*dockerfile : [
                                 "name" : "",
                                 "args : []
                             ],  
                             */
                             env : [:],
                             ports : [ 80, 8080 ]
                        ],
                    ]
                ],
                configures : [
                    class : "EnvVars",
                    files : [
                        "config/gcp-project-key.json"
                    ]
                ]                    
            ],
            git : [
                url : "${env.gitlabSourceRepoSshUrl}",
                branch : "${env.gitlabBranch}"
            ],
            environment : [
                name : "${env.environment}",
                destination : [
                    class : "Gcp.K8s",
                    options : [
                        type : "blue-green",  
                        nodeSelector : "main",
                        /* schedule : "10 * * * *", */
                        expose : [
                            [
                                host : null,
                                paths : [
                                    '/' : 80
                                ],
                                tls : "tlsSecret"
                            ]
                        ],
                        health : [
                            path : "/healthz",
                            port : 80,
                            /* headers: [
                                [
                                    name: "Custom-Header",
                                    value: "Awesome"
                                ]
                            ], */
                            /* exec : [ 
                                'cat /tmp/healthy'
                            ], */
                            delay: [
                                initial : 30,    
                                interval : 60,
                                timeout : 60
                            ],
                            hertz : [
                                successAt : 2,        
                                failureAt : 4
                            ],
                        ],
                        size : [
                            requests : [
                                memory : "200Mi",
                                cpu : "750m"
                            ],
                            requests : [
                                memory : "200Mi",
                                cpu : "750m"
                            ],    
                        ],
                        env : [
                            TZ : "America/Sao_Paulo"
                        ]
                    ]
                ]
            ],
            tests : [
                smoke : [ 
                    class : "Newman",
                ],
            ]
        ]        
    ], 
    this, 
    Jenkins.instance
).run()
