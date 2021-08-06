Bug with the `attr:` directive in `setup.cfg` since the version 0.5.0 of
`build`.

The bug is still present in the current version of `build` (0.6.0.post1).

How to reproduce the bug
========================

~~~~~~~
python -m venv venv-last
source venv-last/bin/activate
pip install -U build
python -m build
~~~~~~~

The error is the following:

~~~~~~~
Creating tar archive
removing 'build_attr_bug-0.1' (and everything under it)
Traceback (most recent call last):
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 387, in _parse_attr
    return getattr(StaticModule(module_name), attr_name)
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 25, in __init__
    with open(spec.origin) as strm:
AttributeError: 'NoneType' object has no attribute 'origin'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/gian/python/build_attr_bug/venv_last/lib/python3.9/site-packages/pep517/in_process/_in_process.py", line 349, in <module>
    main()
  File "/home/gian/python/build_attr_bug/venv_last/lib/python3.9/site-packages/pep517/in_process/_in_process.py", line 331, in main
    json_out['return_val'] = hook(**hook_input['kwargs'])
  File "/home/gian/python/build_attr_bug/venv_last/lib/python3.9/site-packages/pep517/in_process/_in_process.py", line 117, in get_requires_for_build_wheel
    return hook(config_settings)
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/build_meta.py", line 154, in get_requires_for_build_wheel
    return self._get_build_requires(
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/build_meta.py", line 135, in _get_build_requires
    self.run_setup()
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/build_meta.py", line 150, in run_setup
    exec(compile(code, __file__, 'exec'), locals())
  File "setup.py", line 1, in <module>
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/__init__.py", line 153, in setup
    return distutils.core.setup(**attrs)
  File "/usr/lib/python3.9/distutils/core.py", line 121, in setup
    dist.parse_config_files()
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/dist.py", line 778, in parse_config_files
    parse_configuration(self, self.command_options,
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 157, in parse_configuration
    meta.parse()
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 463, in parse
    section_parser_method(section_options)
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 436, in parse_section
    self[name] = value
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 220, in __setitem__
    value = parser(value)
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 557, in _parse_version
    version = self._parse_attr(value, self.package_dir)
  File "/tmp/build-env-46kjbmpx/lib/python3.9/site-packages/setuptools/config.py", line 390, in _parse_attr
    module = importlib.import_module(module_name)
  File "/usr/lib/python3.9/importlib/__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1030, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1007, in _find_and_load
  File "<frozen importlib._bootstrap>", line 984, in _find_and_load_unlocked
ModuleNotFoundError: No module named 'bug'
* Creating venv isolated environment...
* Installing packages in isolated environment... (setuptools>=42, wheel)
* Getting dependencies for sdist...
* Building sdist...
* Building wheel from sdist
* Creating venv isolated environment...
* Installing packages in isolated environment... (setuptools>=42, wheel)
* Getting dependencies for wheel...

ERROR Backend subproccess exited when trying to invoke get_requires_for_build_wheel
~~~~~~~

The full output is in the ``build_last.out`` file.

The bug seems to appear the first time with the version 0.5.0: ::

~~~~~~~
python -m venv venv-0.5.0
source venv-0.5.0/bin/activate
pip install -r requirements_build_0.5.0.txt
python -m build
~~~~~~~

The output is in the ``build_0.5.0.out`` file.

Expected behaviour
==================

~~~~~~~
python -m venv venv-0.4.0
source venv-0.4.0/bin/activate
pip install -r requirements_build_0.4.0.txt
python -m build
~~~~~~~

The output is in the ``build_0.4.0.out`` file.
