# RabbitMQ Guide

## Visit guest user from network

	cat /etc/rabbitmq/enabled_plugins
	[rabbitmq_management]. # ENABLE RABBITMQ MANAGEMENT
	cat /etc/rabbitmq/rabbitmq.config
	[{rabbit, [{loopback_users, []}]}]. # DISABLE ALL USER LOOPBACK
	cat /etc/rabbitmq/rabbitmq-env.conf
	RABBITMQ_NODE_IP_ADDRESS=IP_ADDR # CAN BE VISIT FROM NETWORK