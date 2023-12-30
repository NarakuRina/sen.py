==============================
Sen'Py Visual Novel Engine
==============================

https://www.rina.work/project/senpy

Sen'Py được phát triển dựa trên Ren'Py.


Bắt Đầu
===============

Ren'Py ()nền gốc của Sen'Py) dựa trên một số mô-đun Python được viết bằng Cython và C. Với những thay đổi đối với Ren'Py mà chỉ liên quan đến các mô-đun Python, bạn có thể sử dụng những mô-đun tại bản nightly build mới nhất. Nếu không, bạn sẽ phải tự compile các mô-đun này.

Các development script được mặc định một nền tảng giống như POSIX. Các tập lệnh sẽ chạy
trên linux hay macOS, và có thể chạy được trên Windows bằng cách sử dụng các environment ví dụ như là MSYS.

Nightly Build
-------------

Bản nightly build chỉ có ở bản gốc Ren'Py, bản fork Sen'Py không hỗ trợ.

Compiling the Modules
----------------------

Xây dựng các mô-đun yêu cầu bạn đã cài đặt nhiều dependencies trên hệ thống của bạn. Đối với Ubuntu và Debian, những dependencies này có thể được cài đặt với lệnh::

    sudo apt install virtualenvwrapper python3-dev libavcodec-dev libavformat-dev \
        libswresample-dev libswscale-dev libharfbuzz-dev libfreetype6-dev libfribidi-dev libsdl2-dev \
        libsdl2-image-dev libsdl2-gfx-dev libsdl2-mixer-dev libsdl2-ttf-dev libjpeg-dev

Sen'Py cần có SDL_image phiên bản 2.6 trở lên. Nếu distribution bạn đang sử dụng không bao gồm mô-đun này, bạn sẽ cần phải tải nó tại:

    https://github.com/libsdl-org/SDL_image/tree/SDL2

Bạn nên cài đặt các mô-đun Sen'Py vào Python virtualenv. Để tạo một virtualenv mới, hãy mở một terminal mới và dùng lệnh::

    . /usr/share/virtualenvwrapper/virtualenvwrapper.sh
    mkvirtualenv renpy

Để trở lại VirtualEnv này vào lần tới, gỗ::

    . /usr/share/virtualenvwrapper/virtualenvwrapper.sh
    workon renpy

Sau khi kích hoạt virtualenv, cài đặt các dependencies bổ sung::

    pip install -U cython future six typing pefile requests ecdsa

Sau đó, cài đặt pygame_sdl2 bằng cách dùng lệnh::

    git clone https://www.github.com/renpy/pygame_sdl2
    pushd pygame_sdl2
    python setup.py install
    python setup.py install_headers
    popd

Tiếp theo, đặt RENPY_DEPS_INSTALL thành \:-separated (\;-separated cho Windows) danh sách các đường dẫn chứa các dependencies, và RENPY_CYTHON qua tên của lệnh cython sau::

    export RENPY_DEPS_INSTALL="/usr:/usr/lib/$(gcc -dumpmachine)/"
    export RENPY_CYTHON=cython

Cuối cùng, chạy setup.py trong thư mục ``module`` của Sen'Py để compile và cài đặt các mô-đun hỗ trợ Sen'Py::

    pushd module
    python setup.py install
    popd

Sen'Py sẽ được cài đặt trên virtualenv đang chạy. Nó có thể được chạy bằng lệnh::

    python renpy.py


Tài liệu
=============

Build
--------

Building the documentation requires Ren'Py to work. You'll either need to
link in a nightly build, or compile the modules as described above. You'll
also need the `Sphinx <https://www.sphinx-doc.org>`_ documentation generator.
If you have pip working, install Sphinx using::

    pip install -U sphinx sphinx_rtd_theme sphinx_rtd_dark_mode

Once Sphinx is installed, change into the ``sphinx`` directory inside the
Ren'Py checkout and run::

    ./build.sh

Format
------

Ren'Py's documentation consists of reStructuredText files found in sphinx/source, and
generated documentation found in function docstrings scattered throughout the code. Do
not edit the files in sphinx/source/inc directly, as they will be overwritten.

Docstrings may include tags on the first few lines:

\:doc: `section` `kind`
    Indicates that this function should be documented. `section` gives
    the name of the include file the function will be documented in, while
    `kind` indicates the kind of object to be documented (one of ``function``,
    ``method`` or ``class``. If omitted, `kind` will be auto-detected.
\:name: `name`
    The name of the function to be documented. Function names are usually
    detected, so this is only necessary when a function has multiple aliases.
\:args: `args`
    This overrides the detected argument list. It can be used if some arguments
    to the function are deprecated.

For example::

    def warp_speed(factor, transwarp=False):
        """
        :doc: warp
        :name: renpy.warp_speed
        :args: (factor)

        Exceeds the speed of light.
        """

        renpy.engine.warp_drive.engage(factor)


Translating
===========

For best practices when it comes to translating the launcher and template
game, please read:

https://lemmasoft.renai.us/forums/viewtopic.php?p=321603#p321603


Contributing
============

For bug fixes, documentation improvements, and simple changes, just
make a pull request. For more complex changes, it might make sense
to file an issue first so we can discuss the design.

License
=======

For the complete licensing terms, please read:

https://www.renpy.org/doc/html/license.html
