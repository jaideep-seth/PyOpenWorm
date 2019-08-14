.. _bittorrent:


BitTorrent client for P2P filesharing
=====================================


1. **Download** desired contents:

* A `~PyOpenWorm.data_trans.local_file_ds.LocalFileDataSource` created and stored within the local graph store contains a `torrent_file_name <https://github.com/openworm/PyOpenWorm/pull/424/files#diff-f837dedc6cde6b8c62975ac3b9ed4efe>`_ `~PyOpenWorm.datasource.Informational`. This refers to the torrent containing the location of the desired contents on the BitTorrent. A ``torrent`` is used to locate files on the File System [ `BEP 3 <http://www.bittorrent.org/beps/bep_0003.html>`_ ]. A `~PyOpenWorm.datasource.DataSource` defines attributes about the contents that it represents.


|

  Module ``t`` describes the ``DataSource`` attributes::

        def pow_data(ns):
            ns.context.add_import(ConnectomeCSVDataSource.definition_context)
            ns.context(ConnectomeCSVDataSource)(
            key = '2000_connections',
            csv_file_name = 'connectome.csv',
            torrent_file_name = 'd9da5ce947c6f1c127dfcdc2ede63320.torrent'
         )

|

  The ``DataSource`` can be created and stored on the local graph with::

      $ pow save t



|

  The ``DataSource`` identifier can be used to see contents stored in the local graph with::
   
  $ pow source show ConnectomeCSVDataSource:2000_connections


  ConnectomeCSVDataSource
         CSV file name: 'connectome.csv'

         File name: 'connectome.csv'

         Torrent file name: 'd9da5ce947c6f1c127dfcdc2ede63320.torrent' 
 
* The `BitTorrentDataSourceDirLoader <https://github.com/openworm/PyOpenWorm/pull/449/files>`_ class inherits from the `~PyOpenWorm.datasource_loader.DataSourceDirLoader` and overrides its `load ()`  method. `Google Drive <https://en.wikipedia.org/wiki/Google_Drive>`_ stores the ``torrents`` uploaded by other researchers. ``load()`` fetches the ``torrent`` refered to in ``torrent_file_name`` of the ``DataSource``, performs `~PyOpenWorm.datasource.DataTranslator` from one form to another and then adds the ``torrent`` to the `BitTorrent Client <https://github.com/openworm/bt-gsoc-2019>`_ for downloading its contents.

|

 This ``BitTorrent Client`` is `available on PyPI <https://pypi.org/project/torrent-client/>`_ and is included in the `PyOpenWorm setup <https://github.com/openworm/PyOpenWorm/blob/dev/setup.py>`_.

|

  To install separately::

  $ pip install torrent-client


  For reference, use the `torrent-client repository <https://github.com/jaideep-seth/Torrent_client_gsoc19>`_  and its usage information with::

  $ torrent_cli.py -h

|

 The ``DataSourceDirLoader`` attribute - ``base_directory``, which is set in the ``BitTorrentDataSourceDirLoader`` constructor is where both the ``torrent`` and its contents are downloaded::

  content = BitTorrentDataSourceDirLoader("./")



* Within the `.pow <https://github.com/openworm/PyOpenWorm/blob/dev/docs/command.rst>`_ directory we have the `credentials.json and token.pickle <https://github.com/openworm/OpenWormData/pull/4>`_ these are for authentication of the Google Drive. For the purpose of access control the ``client_secret`` required by ``credentials.json`` will only be shared by PyOpenWorm maintainers.

|
	
* The ``torrent`` file name is the `MD5 message digest <https://en.wikipedia.org/wiki/MD5>`_ of its contents. If the hash of the downloaded contents is the same as its ``torrent`` name the data is unaltered.


|


  Data-Integrity is to be checked after 100% download completion::

  $ python3 integrity.py 'd9da5ce947c6f1c127dfcdc2ede63320.torrent' 'Merged_Nuclei_Stained_Worm.zip'


2. **Upload** your contents:

- On an AWS EC2 instance is running a Nginx WSGI and a Flask Server to accept .zip content file uploads. Visit this Elastic IP address [13.235.204.78] to upload your files by browsing through your filesystem and then clicking the ``Submit Query button``.





- This will create a ``torrent`` and ``seed`` your contents in parts, to other peers on the BitTorrent network. Content can then be downloaded as described above.

