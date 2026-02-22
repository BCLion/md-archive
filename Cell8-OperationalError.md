🧠 Architect is synthesizing Wormhole intelligence...
🛠️ Worker (DeepSeek) is calculating Koopman Dynamics...
✅ Prediction Successful!
---------------------------------------------------------------------------
OperationalError                          Traceback (most recent call last)
/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in _exec_single_context(self, dialect, context, statement, parameters)
   1966                 if not evt_handled:
-> 1967                     self.dialect.do_execute(
   1968                         cursor, str_statement, effective_parameters, context

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/default.py in do_execute(self, cursor, statement, parameters, context)
    951     def do_execute(self, cursor, statement, parameters, context=None):
--> 952         cursor.execute(statement, parameters)
    953 

OperationalError: SSL connection has been closed unexpectedly


The above exception was the direct cause of the following exception:

OperationalError                          Traceback (most recent call last)
/tmp/ipykernel_110/3097716149.py in <cell line: 0>()
     54     with engine.connect() as conn:
     55         # Use ON CONFLICT to avoid the UniqueViolation error
---> 56         conn.execute(text("""
     57             INSERT INTO task_logs (agent_name, task, result, status)
     58             VALUES ('DeepSeek-Quant', 'Wormhole-KNF-Run', :res, 'SUCCESS');

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in execute(self, statement, parameters, execution_options)
   1417             raise exc.ObjectNotExecutableError(statement) from err
   1418         else:
-> 1419             return meth(
   1420                 self,
   1421                 distilled_parameters,

/usr/local/lib/python3.12/dist-packages/sqlalchemy/sql/elements.py in _execute_on_connection(self, connection, distilled_params, execution_options)
    525             if TYPE_CHECKING:
    526                 assert isinstance(self, Executable)
--> 527             return connection._execute_clauseelement(
    528                 self, distilled_params, execution_options
    529             )

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in _execute_clauseelement(self, elem, distilled_parameters, execution_options)
   1639             linting=self.dialect.compiler_linting | compiler.WARN_LINTING,
   1640         )
-> 1641         ret = self._execute_context(
   1642             dialect,
   1643             dialect.execution_ctx_cls._init_compiled,

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in _execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
   1844             return self._exec_insertmany_context(dialect, context)
   1845         else:
-> 1846             return self._exec_single_context(
   1847                 dialect, context, statement, parameters
   1848             )

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in _exec_single_context(self, dialect, context, statement, parameters)
   1984 
   1985         except BaseException as e:
-> 1986             self._handle_dbapi_exception(
   1987                 e, str_statement, effective_parameters, cursor, context
   1988             )

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in _handle_dbapi_exception(self, e, statement, parameters, cursor, context, is_sub_exec)
   2361             elif should_wrap:
   2362                 assert sqlalchemy_exception is not None
-> 2363                 raise sqlalchemy_exception.with_traceback(exc_info[2]) from e
   2364             else:
   2365                 assert exc_info[1] is not None

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/base.py in _exec_single_context(self, dialect, context, statement, parameters)
   1965                             break
   1966                 if not evt_handled:
-> 1967                     self.dialect.do_execute(
   1968                         cursor, str_statement, effective_parameters, context
   1969                     )

/usr/local/lib/python3.12/dist-packages/sqlalchemy/engine/default.py in do_execute(self, cursor, statement, parameters, context)
    950 
    951     def do_execute(self, cursor, statement, parameters, context=None):
--> 952         cursor.execute(statement, parameters)
    953 
    954     def do_execute_no_params(self, cursor, statement, context=None):

OperationalError: (psycopg2.OperationalError) SSL connection has been closed unexpectedly

[SQL: 
            INSERT INTO task_logs (agent_name, task, result, status) 
            VALUES ('DeepSeek-Quant', 'Wormhole-KNF-Run', %(res)s, 'SUCCESS');
            
            UPDATE agent_states 
            SET xp = xp + 10 
            WHERE agent_name = 'DeepSeek-Quant';
        ]
[parameters: {'res': 'Not enough data points (0) for DMD prediction. Need at least 5.\n{"prediction": -1.0, "confidence": 0.0, "data_points": 0, "top_symbol": "N/A"}\n'}]
(Background on this error at: https://sqlalche.me/e/20/e3q8)
